pipeline {
  agent any
  environment {
    ECR_REPO = '905418306644.dkr.ecr.eu-central-1.amazonaws.com/todo-app'
  }
  stages {
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $ECR_REPO .'
      }
    }
    stage('Push to ECR') {
      steps {
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-creds']]) {
          sh 'aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin 905418306644.dkr.ecr.eu-central-1.amazonaws.com'
          sh 'docker push $ECR_REPO'
        }
      }
    }
    stage('Deploy to K8s') {
      steps {
        sh 'kubectl apply -f k8s/deployment.yaml'
        sh 'kubectl apply -f k8s/service.yaml'
      }
    }
  }
}
