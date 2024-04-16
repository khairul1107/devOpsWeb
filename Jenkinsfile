pipeline {
    agent any
    tools {
        maven 'local_maven'
    }
    stages {
        stage ('Build') {
            steps {
                // Using 'bat' instead of 'sh' for Windows compatibility
                bat 'mvn clean package'
            }
            post {
                success {
                    echo "Archiving the Artifacts"
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Tomcat server') {
            steps {
                script {
                    // Read properties from file
                    readProp = readProperties file: 'build.properties'
                    echo "This is running on ${readProp['deploy.type']}"
                    // Deploy war file to Tomcat server
                    deploy adapters: [tomcat7(credentialsId: 'a1e468f8-a2d4-4c90-a151-d60dfb22c489', path: '', url: 'http://localhost:8181/')], contextPath: null, war: "**/${readProp['deploy.app.name']}.war"
                }
            }
        }
    }
}
