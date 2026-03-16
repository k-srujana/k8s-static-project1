pipeline {

 agent any

 environment {
  IMAGE = "srujana97/static-devops"
  TAG = "${BUILD_NUMBER}"
 }

 stages {

  stage('Build Docker Image') {
   steps {
    bat "docker build -t %IMAGE%:%TAG% ."
   }
  }

  stage('Push Image') {
   steps {
    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
     usernameVariable: 'USER',
     passwordVariable: 'PASS')]) {

     bat "echo %PASS% | docker login -u %USER% --password-stdin"
     bat "docker push %IMAGE%:%TAG%"
    }
   }
  }

  stage('Deploy') {
   steps {
    bat "helm upgrade static-app static-app --set image.repository=srujana97/static-devops --set image.tag=%TAG%"
   }
  }

 }

}