pipeline {
    agent any

    environment {
        SONARQUBE_URL = "http://13.53.123.95:9000/"
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    git branch: "develop", 
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
            steps {
                script {
                    deploy adapters: [tomcat7(
                        credentialsId: 'tomcat-credentials', 
                        path: '', 
                        url: 'http://13.51.165.114:8080/'
                    )], 
                    contextPath: 'numbers-game', war: 'target/*.war'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build, Testing, SonarQube Analysis, and Deployment Successful!'
            emailext(
                subject: "Build Success: ${env.JOB_NAME}",
                body: "Build #${env.BUILD_NUMBER} was successful.\nCheck the details at: ${env.BUILD_URL}",
                to: "georgesomina91@gmail.com, janecollins171993@gmail.com, kehinde_jimoh@yahoo.co.uk"
            )
        }
        failure {
            echo '❌ Build Failed! Check logs for issues.'
        }
    }  // ✅ Correctly closed 'post' block

}  // ✅ Correctly closed 'pipeline' block
