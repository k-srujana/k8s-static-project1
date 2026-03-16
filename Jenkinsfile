pipeline {

 agent any

 environment {
  IMAGE = "k-srujana/static-devops"
  TAG = "${BUILD_NUMBER}"
 }

 stages {

  stage('Clone') {
   steps {
    git 'https://github.com/k-srujana/k8s-static-project1.git'
   }
  }

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

     bat "docker login -u %USER% -p %PASS%"
     bat "docker push %IMAGE%:%TAG%"
    }
   }
  }

  stage('Deploy') {
   steps {
    bat "helm upgrade static-app static-app --set image.tag=%TAG%"
   }
  }

 }

}