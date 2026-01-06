pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Upload to Artifactory using curl') {
            environment {
                ART_URL = "http://65.1.148.156:8081/artifactory/maven-expo"
            }
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'jfrog',
                    usernameVariable: 'ART_USER',
                    passwordVariable: 'ART_PASS'
                )]) {
                    sh '''
                    FILE=$(ls target/*.jar | head -n 1)
                    FILENAME=$(basename "$FILE")

                    echo "Uploading $FILENAME"

                    curl -X PUT -u $ART_USER:$ART_PASS \
                    -T "$FILE" \
                    "$ART_URL/$FILENAME"
                    '''
                }
            }
        }
    }
}
