pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {
        stage('Build & Deploy') {
            steps {
                script {
                    def server = Artifactory.newServer(
                        url: 'http://65.1.148.156:8081/artifactory',
                        credentialsId: 'jfrog-admin'
                    )

                    def rtMaven = Artifactory.newMavenBuild()
                    rtMaven.tool = 'maven'

                    rtMaven.deployer(
                        server: server,
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
