pipeline{
    agent any
    tools{
        maven 'local_maven'
    }
    stages{
        stage ('Build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    echo "Archiving the Artifacts"
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to tomcat server') {
            steps{
                script{
                    readProp = readProperties file: 'build.properties'
                }
                echo "This is running on ${readProp['deploy.type']}"
                deploy adapters: [tomcat7(credentialsId: 'a1e468f8-a2d4-4c90-a151-d60dfb22c489', path: '', url: 'http://localhost:8181/')], contextPath: null, war: '**/*.war'
        }
    }
}
