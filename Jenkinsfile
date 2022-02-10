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
                ws('D:\\DEVOPS\\OneDrive_1_1-31-2022\\Hello-World-JAVA-master\\Hello-World-JAVA-master'){
		    def scannerHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation';
                    withSonarQubeEnv('Sonar') { 
      		    bat "${scannerHome}/bin/sonar-scanner"
                    }
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
