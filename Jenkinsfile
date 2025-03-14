pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // If repo is private, add credentialsId
                    git branch: 'dev', 
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

        stage('Deploy') {
            steps {
                echo 'Deploying Application...'
            }
        }
    }

    // ✅ Added post section for notifications & cleanup
    post {
        success {
            echo '✅ Build and Deployment Successful!'
        }
        failure {
            echo '❌ Build Failed! Check logs for issues.'
        }
    }
}
