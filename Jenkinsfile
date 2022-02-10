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
                git 'https://github.com/Apurva001/DevopsforQA2021NAGP.git, credentialsId: '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', branch: master'
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
                withSonarQubeEnv('sonar') { 	
                bat "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"
                }
            }
        }

        stage("Quality gate") {
      steps {
          waitForQualityGate abortPipeline: true
            }
        }
     
     stage('Build Artifacts') {
            server = Artifactory.server "artifactory-server"
            buildInfo = Artifactory.newBuildInfo()
            rtMaven = Artifactory.newMavenBuild()
            rtMaven.tool = 'MAVEN3'
            rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'mvn01', server: server
            rtMaven.resolver releaseRepo:'remote-repos', snapshotRepo:'remote-repos', server: server
            rtMaven.run pom: 'pom.xml', goals: 'clean install -Dmaven.test.skip=true', buildInfo: buildInfo
            publishBuildInfo server: server, buildInfo: buildInfo
        }
	    
	  stage("final result"){
      steps{
          bat "echo success"
            }
        }
    }

}
