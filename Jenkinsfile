pipeline {
    agent any

    environment {
	SONARQUBE_URL = "http://13.53.123.174:9000"
	SONARQUBE_TOKEN = credentials("sonarqube-token")
	TOMCAT_CREDS =credentials ("tomcat-credentials")
	TOMCAT_URL = "http://13.60.242.211:8080"

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

 	stage('SonarQube Analysis') {
            steps {
                script {
		      sh '''
			mvn clean verify sonar:sonar \
  			-Dsonar.projectKey=NumbersGuessGame \
  			-Dsonar.projectName='NumbersGuessGame' \
  			-Dsonar.host.url=$SONARQUBE_URL \
  			-Dsonar.token=sqp_$SONARQUBE_TOKEN
			'''
                   
            }
        }
	

        stage('Deploy') {
            steps {
                echo 'Deploying Application...'
                
                // Deploying using Tomcat plugin
                deploy adapters: [tomcat7(credentialsId: 'tomcat-credentials', path: '', url: 'http://13.60.242.211:8080')], 
                    contextPath: null, war: '**/*.war'
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
        
