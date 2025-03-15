pipeline {
    agent any  {
    }
        stages{
            stage('Build') {
                steps {
                    sh 'mvn clean package'
                }
                post{
                    success{
                        echo 'Archiving the Artifacts'
                        archiveArtifacts artifacts: '**/target/*.war'
                    }
                }
            }
            stage ('Deploy to tomcat server') {
                steps{
                    steps{
                        deploy adapters: [tomcat7(path: '', url: 'http://51.21.194.75:8080/')], contextPath: null, war: '**/*.war'
                    }
                }
            }
        }
}
