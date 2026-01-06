pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {
        stage('Build & Deploy to Artifactory') {
            steps {
                script {
                    def server = Artifactory.newServer(
                        url: 'http://65.1.148.156:8081/artifactory',
                        credentialsId: 'jfrog'
                    )

                    def rtMaven = Artifactory.newMavenBuild()
                    rtMaven.tool = 'maven'

                    // 👇 BOTH repos must be defined
                    rtMaven.deployer(
                        server: server,
                        releaseRepo: 'maven-release',
                        snapshotRepo: 'maven-expo'
                    )

                    rtMaven.run(
                        pom: 'pom.xml',
                        goals: 'clean install'
                    )
                }
            }
        }
    }
}
