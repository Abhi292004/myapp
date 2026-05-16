pipeline {
  agent any
  environment {
    AWS_REGION   = 'us-east-1'
    ECR_REGISTRY = '123456789.dkr.ecr.us-east-1.amazonaws.com'
    ECR_REPO     = 'myapp'
    IMAGE_TAG    = "${BUILD_NUMBER}"
    K8S_NS       = 'production'
  }
  tools {
    maven 'Maven-3.9'
    jdk   'Java-17'
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            credentialsId: 'github-creds',
            url: 'https://github.com/yourname/myapp.git'
      }
    }
    stage('Build & Test') {
      steps { sh 'mvn clean package' }
      post { always { junit 'target/surefire-reports/*.xml' } }
    }
    stage('Docker Build & Push') {
      steps {
        sh """
          aws ecr get-login-password --region ${AWS_REGION} | \
            docker login --username AWS --password-stdin ${ECR_REGISTRY}
          docker build -t ${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG} .
          docker push ${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG}
        """
      }
    }
    stage('Deploy to EKS') {
      steps {
        sh """
          aws eks update-kubeconfig --name devops-cluster --region ${AWS_REGION}
          kubectl set image deployment/myapp-deployment \
            myapp=${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG} -n ${K8S_NS}
          kubectl rollout status deployment/myapp-deployment -n ${K8S_NS}
        """
      }
    }
  }
}