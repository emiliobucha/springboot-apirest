pipeline {
    agent any
    stages {
	stage('Setting up environment variables'){
	  	def AWS_ACCOUNT_CREDENTIALS_ID = 'AWS Jenkins'
		def REGION = 'us-east-1'
		def FILE = 'bucha-artifact-test.zip'
		def BUCKET = 'semperti-rapientrega-development-s3-backend-artifact'
		def PROJECT = 'bucha-test'
      	}
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
            steps {
		    	withAWS(region:"${REGION}",credentials:"${AWS_ACCOUNT_CREDENTIALS_ID}") {    
			s3Upload(file:"${FILE}", bucket:"${BUCKET}", path:"${PROJECT}/")
		}
            }
        }
    }
}
