pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {
        stage('Build & Deploy to JFrog Artifactory') {
            steps {
                script {
                    def server = Artifactory.newServer(
                        url: 'http://65.1.148.156:8081/artifactory',
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
    }
}
