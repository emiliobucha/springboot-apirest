pipeline {
    agent any
    environment {
        AWS_ACCOUNT_CREDENTIALS_ID = 'AWS Jenkins'
        REGION = 'us-east-1'
        FILE = 'bucha-artifact-test.zip'
        BUCKET = 'semperti-rapientrega-development-s3-backend-artifact'
        PROJECT = 'bucha-test'
    }
    stages {

        stage('Build') {
            steps {
                sh "mvn clean package -B -DskipTests"
                sh "zip ${FILE} target/*.jar Dockerfile"
            }

            post {
                success {
                    archiveArtifacts "${FILE}"
                }
            }
        }
        stage('Upload') {
            step {
                sh "echo Uploading... pepito"
            }
            scripts {
                withAWS(region:"${REGION}",credentials:"${AWS_ACCOUNT_CREDENTIALS_ID}") {    
                    s3Upload(file:"${FILE}", bucket:"${BUCKET}", path:"${PROJECT}/")
                }
            }
        }
    }
}
