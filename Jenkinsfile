pipeline {

  agent any
  stages {

    stage("Git Checkout") {
      steps {
        script {
           sh "git clone https://github.com/gopinitw/cicd-java-maven-project.git"
        }
      }
    }

    stage("Maven Build") {
      steps {
        script {
          sh "mvn clean install"
        }
      }
    }

    stage("Build & Push Docker Image") {
      steps {
        script {
          sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          sh "docker build -t dash18/cicd-java-maven ."
          sh "docker push dash18/cicd-java-maven"
        }
      }
    }

    stage("Apply the Kubernetes files") {
      steps {
        script {
          sh "kubectl apply -f kubernetes/ "
        }
      }
    }
  }
  post {
    always {
      script {
        if (currentBuild.currentResult == 'FAILURE') {
          step([$class: 'Mailer', notifyEveryUnstableBuild: true, recipients: "Test@test.com", sendToIndividuals: true])
        }
      }
    }
  }
}
