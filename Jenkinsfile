pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('algow-dockerhub-token')
		AWS_ACCESS_KEY_ID     = credentials('algow-aws-secret-key-id')
                AWS_SECRET_ACCESS_KEY = credentials('algow-aws-secret-access-key')

		AWS_S3_BUCKET = "algow-belt2-artifacts-123456"
		AWS_REGION = "us-east-1"
                ARTIFACT_NAME = "Dockerrun.aws.json"
                AWS_EB_APP_NAME = "runaway-algow"
                AWS_EB_APP_VERSION = "${BUILD_ID}"
                AWS_EB_ENVIRONMENT = "Runawayalgow-env"
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t algow/runaway:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push algow/runaway:latest'
			}
		}

		stage('Deploy') {
            steps {

                sh 'aws elasticbeanstalk create-application-version --region us-east-1 --application-name $AWS_EB_APP_NAME --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$ARTIFACT_NAME'

                sh 'aws elasticbeanstalk update-environment --region us-east-1 --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT --version-label $AWS_EB_APP_VERSION'
            
                
            }
        }
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
