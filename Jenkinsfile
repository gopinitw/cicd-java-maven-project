pipeline {

  agent any
    environment {
    DOCKERHUB_CREDENTIALS=credentials('dockerhub') // Create a credentials in jenkins using your dockerhub username and token from https://hub.docker.com/settings/security
  }

  stages {

    // stage("Git Checkout") {
    //    steps {
    //     script {
    //        sh "git clone https://github.com/gopinitw/cicd-java-maven-project.git"
    //      }
    //   }
    //  }

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
          sh "docker build -t gopigundeboyina/mavencicd ."
          sh "docker push gopigundeboyina/mavencicd"
        }
      }
    }

    stage("Apply the Kubernetes files") {
      steps {
        script {
          sh "aws eks describe-cluster --name eksdemo1 --region us-east-1"
          sh "kubectl apply -f kubernetes/Deployment.yaml"
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
