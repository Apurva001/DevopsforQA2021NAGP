pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven 'MAVEN3'
    }

    stages {
        stage('code checkout') {
            steps {
                bat "echo hello"
            }

        }

        stage('code build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/Apurva001/DevopsforQA2021NAGP.git'
                // Run Maven on a Unix agent.
                bat "mvn clean"
            }

        }

        stage('Unit test') {
            steps {
                // Run Maven on a Unix agent.
                bat "mvn test"
            }

        }
        
        stage('Sonar analysis') {
            steps {
                withSonarQubeEnv(credentialsId: 'Sonar_Token') {
				waitForQualityGate abortPipeline: false, credentialsId: 'Sonar_Token'
				}
            }

        }
    } 
  
}
