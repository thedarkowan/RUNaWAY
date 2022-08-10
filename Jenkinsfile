pipeline {

  agent any

  environment {
    DOCKERHUB_CREDENTIALS=credentials('belt-token')
    AWS_ACCESS_KEY_ID     = credentials('rawanzabidi-aws-secret-key-id')
    AWS_SECRET_ACCESS_KEY = credentials('rawanzabidi-aws-secret-access-key')
    ARTIFACT_NAME = 'Dockerrun.aws.json'
    AWS_S3_BUCKET = 'rawanzabidi-belt2-artifacts-123456'
    AWS_EB_APP_NAME = 'belt-EB'
    AWS_EB_ENVIRONMENT_NAME = 'Belteb-env'
    AWS_EB_APP_VERSION = "${BUILD_ID}"
  }

  stages {

    stage('Build') {

      steps {
        sh 'docker build -t rawanzabidi/runaway:latest .'
      }
    }

    stage('Login') {

      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }

    stage('Push') {

      steps {
        sh 'docker push rawanzabidi/runaway:latest'
      }
    }

    stage('Deploy') {
            steps {
                sh 'aws configure set region us-east-1'
                sh 'aws elasticbeanstalk create-application-version --application-name $AWS_EB_APP_NAME --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$ARTIFACT_NAME'
                sh 'aws elasticbeanstalk update-environment --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT_NAME --version-label $AWS_EB_APP_VERSION'
            }
  }
    }
  post {
    always {
      sh 'docker logout'
    }
  }

}
