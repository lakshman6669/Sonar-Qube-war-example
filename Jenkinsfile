pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
        SCANNER_HOME = tool 'SonarScanner'
    }

    stages {
        stage('Build & Deploy to JFrog Artifactory') {
            steps {
                script {
                    def server = Artifactory.newServer(
                        url: 'http://3.110.155.145:8081/artifactory',
                        credentialsId: 'jfrog'
                    )

                    def rtMaven = Artifactory.newMavenBuild()
                    rtMaven.tool = 'maven'

                    rtMaven.deployer(
                        server: server,
                        snapshotRepo: 'pipeline-repo',
                        releaseRepo: 'pipeline-repo'
                    )

                    rtMaven.run(
                        pom: 'pom.xml',
                        goals: 'clean deploy'
                    )
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar_install') {
                    sh """
                    ${SCANNER_HOME}/bin/sonar-scanner \
                    -Dsonar.projectName=Validate \
                    -Dsonar.projectKey=Validate \
                    -Dsonar.host.url=http://13.232.129.239:9000
                    """
                }
            }
        }
    }
}
