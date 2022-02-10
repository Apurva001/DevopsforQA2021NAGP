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

                withSonarQubeEnv('Sonar') { 
		    bat "${scannerHome}/bin/sonar-scanner.bat"
			-D sonar.login=admin \
      			-D sonar.password=admin \
      			-D sonar.projectKey=Devops Sample Application \
     			-D sonar.exclusions=vendor/**,resources/**,**/*.java \
      			-D sonar.host.url=http://localhost:9000/sonar"
		    bat 'sonar:sonar'
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
