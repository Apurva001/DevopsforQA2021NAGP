node {
 
    withEnv(["PATH+MAVEN=${tool 'MAVEN3'}bin"]) {
 
        stage('Checkout') {
            def branch = env.gitlabBranch
            env.branch = branch
            git url: 'https://github.com/Apurva001/DevopsforQA2021NAGP.git', credentialsId: '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', branch: branch
        }
 
        stage('Test') {
            def pom = readMavenPom file: 'pom.xml'
            print "Build: " + pom.version
            env.POM_VERSION = pom.version
            sh 'mvn clean test -Dmaven.test.failure.ignore=true'
            junit '**/target/surefire-reports/TEST-*.xml'
            currentBuild.description = "v${pom.version} (${env.branch})"
        }
 
        stage('QA') {
            withSonarQubeEnv('sonar') {
                sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
            }
        }
 
        stage('Build') {
            def server = Artifactory.server "artifactory-server"
            def buildInfo = Artifactory.newBuildInfo()
            def rtMaven = Artifactory.newMavenBuild()
            rtMaven.tool = 'MAVEN3'
            rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'mvn01', server: server
            rtMaven.resolver releaseRepo:'remote-repos', snapshotRepo:'remote-repos', server: server
            rtMaven.run pom: 'pom.xml', goals: 'clean install -Dmaven.test.skip=true', buildInfo: buildInfo
            publishBuildInfo server: server, buildInfo: buildInfo
        }

 
    }
}
