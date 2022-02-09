pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven 'MAVEN3'
    }

    stages {
        stage('code checkout') {
            steps {
                sh "echo hello"
            }

        }

        stage('code build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn clean"
            }

        }

        stage('Unit test') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn test"
            }

        }
    } 
  
}
