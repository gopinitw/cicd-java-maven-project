pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    AWS_DEFAULT_REGION = 'us-east-1'
    BUILD_NUMBER = env.BUILD_NUMBER
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
    stage("Apply the Kubernetes files") {
      steps {
        script {
          withCredentials([[
            $class: 'AmazonWebServicesCredentialsBinding',
            credentialsId: 'awskeys'
          ]]) {
            sh 'aws eks update-kubeconfig --region us-east-1 --name eksdemo1'
            sh 'kubectl get pods'
            // Update image in Deployment.yaml
            sh "sed -i 's|image: .*|image: gopigundeboyina/mavencicd:$BUILD_NUMBER|g' kubernetes/Deployment.yaml"
            
            // Apply updated Deployment.yaml
            sh 'kubectl apply -f kubernetes/Deployment.yaml'
          }
        }
      }
    }
  }
}
