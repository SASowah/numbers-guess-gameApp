pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the Artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy to Tomcat Server') {
            steps {
                deploy adapters: [tomcat7(
                    credentialsId: 'deploy-user',
                    path: '',
                    url: 'http://51.21.194.75:8080/'
                )], contextPath: null, war: '**/target/*.war'
            }
        }
    }
}
