pipeline {
    agent any
    options {
	    withAWS(region:'us-east-1',credentials:'AWS Jenkins')
    }
    stages {
        stage('Build') {
            steps {
                sh "mvn clean package -B -DskipTests"
                sh "zip bucha-artifact-test.zip target/*.jar Dockerfile"
            }

            post {
                success {
                    archiveArtifacts 'bucha-artifact-test.zip'
                }
            }
        }
        stage('Upload') {
            steps {
                s3Upload(file:'bucha-artifact-test.zip', bucket:'semperti-rapientrega-development-s3-backend-artifact', path:'bucha-artifact-test.zip')
            }
        }
    }
}
