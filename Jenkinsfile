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
                kubeconfig(caCertificate: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJTGJyOFlCbStvakF3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TkRBek16QXdOekUyTkRWYUZ3MHpOREF6TWpnd056SXhORFZhTUJVeApFekFSQmdOVkJBTVRDbXQxWW1WeWJtVjBaWE13Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLCkFvSUJBUUR2Mi9zYktBZ1prQ0hsUHFlRjZJaWU5QnY4cHhET1pNdTBkV1l1akdjTmdHT2Z1RTMxQnA2SjB0ZE8KQ3RsNzFpQW5td3hQd3pNMFFUdXQzazJSc3pCWlprNHBEdE56dmVxQ0RLU1FVZmFzb2MxdmdYY24ydlJpWVRFUgozM2lpZTE5RUc0T0g4UjRabnRJbnMzOWdERlpOS2JBaXhJaUpiQTFJMXMxWVZsYkVCY1VIUkM4aTlOTjBEblZmCmNvbGhrK1l6dThGanBEREFvRjNJN05MUlgvMzRaaWg5RlpvaitIRmNFMGpJMUtzNUI1b1pEMmNodzBwOHkzNGcKZ0FDODdEVFlHUldqQlA5T3k4TFVGK1Qwd1V3aThlcHVJNHJ3c1VSMWtHWkQ0U2xsMDY1OTVkVmNDUk94VCsvVwp5ZXBrMTc5ZHpzOCtEMytKL083dTRKWGlLQmJoQWdNQkFBR2pXVEJYTUE0R0ExVWREd0VCL3dRRUF3SUNwREFQCkJnTlZIUk1CQWY4RUJUQURBUUgvTUIwR0ExVWREZ1FXQkJSYzJZTG5yOXAzNjBKQ0dRQzJrRGRmaXFaVGhUQVYKQmdOVkhSRUVEakFNZ2dwcmRXSmxjbTVsZEdWek1BMEdDU3FHU0liM0RRRUJDd1VBQTRJQkFRQ2RrZzI2TUdrUgplYWc5a3dKQXA0U0VTMXJ0UlhBOXhOYTJkTVpYNEQrTXgzaERxUlZnNnZWTG8wTENVTjhWbEpyQlJaa2V4d2h2Clcrc05vNjM3QTVBWlQ1b0M3WFVPeHk5L2NrWDc3d01tQ3YrY1FWd1phZll1VnlvdHhtV1pWY2N5UVR3Z0t5SUkKSW5oUWVkeHh1UjByNUc0aFUxVGpDc3hVSThWYm8yMHpSbUlKUis5OXR5Tnhic0xtSjEvZG4vYjBqblhaNnFtbgpBZ3d2d2psaktrYlp0Ry9iNDI1K0JyYXVzcHZ5OGZwK0FhU09KbVBkeTFYTUFmY2VHRUNjSEhlRFFVdmUwV3lLClB6V0Y2UlFFd3FyWmNyWEMwVGVUcno5dG90V2t4bGVJcmxUeEhmSEl0cmM0NGhWVnlnN2krc0JORGg0ZDZZYTcKV01sWEpjZ04yTGVGCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K', credentialsId: 'kubeconfig', serverUrl: 'https://627BEF2BDA2DCCB3D82C71D5635E0123.gr7.us-east-1.eks.amazonaws.com') {
                sh 'kubectl apply -f kubernetes/Deployment.yaml'
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
