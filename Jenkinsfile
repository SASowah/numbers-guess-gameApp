pipeline {
    agent any

    environment {
        SONARQUBE_URL = credentials('sonarqube-url') // Stored in Jenkins Credentials
    }

    parameters {
        string(name: 'DEPLOY_ENV', defaultValue: 'dev', description: 'Deployment Environment')
        string(name: 'BRANCH', defaultValue: 'dev', description: 'Git Branch to build')
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    git branch: "${params.BRANCH}", 
                        url: 'https://github.com/SASowah/numbers-guess-gameApp.git'
                }
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('sonarqube-token') { 
                        sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=NumbersGuessGame \
                        -Dsonar.projectName="NumbersGuessGame" \
                        -Dsonar.host.url=${SONARQUBE_URL} \
                        -Dsonar.login=${SONAR_AUTH_TOKEN}
                        '''
                    }
                }
            }
        }

        stage('Deploy to Tomcat') {
            environment {
                TOMCAT_URL = credentials('tomcat-url') // Jenkins Credential Store
            }
            steps {
                script {
                    deploy adapters: [tomcat7(
                        credentialsId: 'tomcat-credentials', 
                        path: '', 
                        url: "${TOMCAT_URL}"
                    )], 
                    contextPath: 'numbers-game', war: 'target/*.war'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build, Testing, SonarQube Analysis, and Deployment Successful!'
        }
        failure {
            echo '❌ Build Failed! Check logs for issues.'
        }
    }
}
