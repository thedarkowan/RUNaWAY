pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('belt-token')
	}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
	
	stages {

		stage('Build') {

			steps {
				sh 'docker build -t rawanzabidi/run:latest .'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push rawanzabidi/run:latest'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
