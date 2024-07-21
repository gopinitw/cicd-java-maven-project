pipeline {
  agent any
  tools {
		maven "mvn"
       	}
  environment {
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
}
}
