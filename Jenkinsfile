pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        echo 'Checking out code...'
      }
    }
    stage('Build') {
      steps {
        echo 'Building application...'
      }
    }
    stage('Test') {
      steps {
        echo 'Running tests...'
      }
    }
    stage('Docker Build') {
      steps {
        echo 'Building Docker image...'
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying application...'
      }
    }
  }
  post {
    success {
      echo 'Pipeline completed successfully!'
    }
    failure {
      echo 'Pipeline failed!'
    }
  }
}