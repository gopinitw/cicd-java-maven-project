pipeline {

  agent any
    environment {
    DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    PATH = "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin:/home/ubuntu/bin/kubectl:$PATH"
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
      sh 'aws eks update-kubeconfig --region us-east-1 --name eksdemo1'
      sh '/var/lib/jenkins/workspace/java/kubectl --kubeconfig=/var/lib/jenkins/workspace/java/config config view'
      sh '/var/lib/jenkins/workspace/java/kubectl --kubeconfig=/var/lib/jenkins/workspace/java/config get pods'
    }

    }
  }
  }
  }
