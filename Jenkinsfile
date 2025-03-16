pipeline {
    agent any

    environment {
        SONARQUBE_URL = "http://13.53.123.174:9000"
        TOMCAT_URL = "http://13.60.242.211:8080"
    }

    parameters {
        string(name: 'BRANCH', defaultValue: 'dev', description: 'Git branch to build')
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
                sh 'mvn clean package'
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
                    withSonarQubeEnv('sonarqube-server') {  
                        sh '''
                        mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=NumbersGuessGame \
                        -Dsonar.projectName="NumbersGuessGame" \
                        -Dsonar.host.url=${SONARQUBE_URL}
                        '''
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'tomcat-credentials', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASS')]) {
                        deploy adapters: [tomcat7(
                            credentialsId: 'tomcat-credentials', 
                            path: '', 
                            url: TOMCAT_URL
                        )], 
                        war: '**/*.war'
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and Deployment Successful!'
        }
        failure {
            echo '❌ Build Failed! Check logs for issues.'
        }
    }
}
