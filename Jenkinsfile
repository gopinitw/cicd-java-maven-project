pipeline {
  agent any
      	tools {
		maven "mvn"
       	}
  environment {
    DOCKERHUB_CREDENTIALS=credentials('DOCKERHUB')
    AWS_DEFAULT_REGION = 'us-east-1'
    BUILD_NUMBER = "${env.BUILD_NUMBER}"
  }
  stages {
    stage("Maven Build") {
      steps {
        script {
          sh "mvn clean install -DskipTests"
        }
      }
    }
	      stage("Build & Push Docker Image") {
      steps {
        script {
          sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          sh "docker build -t gopigundeboyina/mavencicd:$BUILD_NUMBER ."
          sh "docker push gopigundeboyina/mavencicd:$BUILD_NUMBER"
        }
      }
    }
  }
}
