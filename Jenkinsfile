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
                    credentialsId: 'tomcat-credentials', // Add credentials here
                    path: '',
                    url: 'http://51.21.194.75:8080/manager/html'
                )], contextPath: null, war: '**/target/*.war'
            }
        }
    }
}
