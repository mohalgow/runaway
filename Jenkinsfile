pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('algow-dockerhub-token')
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
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}