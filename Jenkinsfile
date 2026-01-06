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
    }
}
