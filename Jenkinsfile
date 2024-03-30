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
  withKubeConfig([credentialsId: 'kubeconfig', serverUrl: 'https://627BEF2BDA2DCCB3D82C71D5635E0123.gr7.us-east-1.eks.amazonaws.com']) {
      sh 'kubectl apply -f kubernetes/Deployment.yaml'
    }

    }
  }
  }
  }
}
