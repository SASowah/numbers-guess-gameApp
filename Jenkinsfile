pipeline {
    agent any    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // If repo is private, add credentialsId
                    git branch: 'develop',
                        url: 'https://github.com/SASowah/numbers-guess-gameApp.git'
                }
            }
        }        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying Application to Tomcat...'
                    deploy adapters: [
                         tomcat9(credentialsId: 'tomcat-cred', path: '', url: "${env.TOMCAT_URL}")
                    ], contextPath: 'numbers-game', war: '**/target/*.war'
                }
            }
        }
    }    // :white_check_mark: Post section for notifications & cleanup
    post {
        success {
            echo ':white_check_mark: Build and Deployment Successful!'
        }
        failure {
            echo ':x: Build Failed! Check logs for issues.'
        }
    }
}


