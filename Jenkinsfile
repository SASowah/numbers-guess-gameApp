pipeline {
    agent any
    parameters {
        string(name: 'DEPLOY_ENV', defaultValue: 'dev', description: 'Deployment Environment')
        string(name: 'BRANCH', defaultValue: 'dev', description: 'Git Branch to build')
        string(name: 'SONAR_INSTANCE', defaultValue: 'SonarQube', description: 'SonarQube Installation Name')
        string(name: 'SONARQUBE_URL', defaultValue: 'http://your-sonarqube-ip:9000/', description: 'SonarQube Server URL')
        string(name: 'SONAR_AUTH_TOKEN', defaultValue: '', description: 'SonarQube Authentication Token')
        string(name: 'TOMCAT_INSTANCE', defaultValue: 'http://your-tomcat-ip:8080/', description: 'Tomcat Server URL')
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
                    withSonarQubeEnv("${params.SONAR_INSTANCE}") {
                        sh """
                        mvn sonar:sonar \\
                        -Dsonar.projectKey=NumbersGuessGame \\
                        -Dsonar.projectName="NumbersGuessGame" \\
                        -Dsonar.host.url=${params.SONARQUBE_URL} \\
                        -Dsonar.login=${params.SONAR_AUTH_TOKEN}
                        """
                    }
                }
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                script {
                    deploy adapters: [tomcat7(
                        credentialsId: 'tomcat-credentials',
                        path: '',
                        url: "${params.TOMCAT_INSTANCE}"
                    )],
                    contextPath: 'numbers-game', war: 'target/*.war'
                }
            }
        }
    }
    post {
        success {
            echo ':white_check_mark: Build, Testing, SonarQube Analysis, and Deployment Successful!'
        }
        failure {
            echo ':x: Build Failed! Check logs for issues.'
        }
    }
}
