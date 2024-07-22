pipeline {
  agent any
  	tools {
		maven "mvn"
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
          sh "docker build -t gopigundeboyina/devcart:$BUILD_NUMBER ."
          sh "docker push gopigundeboyina/devcart:$BUILD_NUMBER"
        }
      }
    }
}
}
