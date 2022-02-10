pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven 'MAVEN3'
    }

    stages {
        stage('Cloning project from git') {
            steps {
		   // Get some code from a GitHub repository
                git 'https://github.com/Apurva001/DevopsforQA2021NAGP.git'
            }
        }

        stage('Code build') {
            steps {
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

        stage('SonarQube analysis') {
		 steps {  
			 script {
            		 scannerHome = tool 'SonarScanner';
       		 	}		 
                    withCredentials([string(credentialsId: 'Sonar_Token', variable: 'sonarLogin')])  { 
      		    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=gs-gradle -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=GS -Dsonar.sources=complete/src/main/ -Dsonar.tests=complete/src/test/ -Dsonar.language=java -Dsonar.java.binaries=."
                    }
                }
            }

        stage("Quality gate") {
      steps {
          waitForQualityGate abortPipeline: true
            }
        }
    }

}
