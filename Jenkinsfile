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
                bat "mvn install"
            }

        }

        stage('Unit test') {
            steps {
                // Run Maven on a Unix agent.
                bat "mvn clean test -Dmaven.test.failure.ignore=true"
                junit '**/target/surefire-reports/TEST-*.xml'
            }

        }

        stage('SonarQube analysis') {
		steps {	 
                withSonarQubeEnv('Sonar') { 	
                bat "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.1.2184:sonar"
                }
            }
        }

        stage("Quality gate") {
      steps {
          waitForQualityGate abortPipeline: true
            }
        }
	    
	  stage("final result"){
      steps{
          bat "echo success"
            }
        }
    }

}
