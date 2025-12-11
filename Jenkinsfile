pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS=credentials('dockerhubID')
    IMAGE_NAME="1ms24mc048/my_webapp"
  }

  stages {
    stage ('Checkout') {
      git (
        url:'https://github.com/1ms24mc048/my_webapp',
        branch:'main',
        credentialsID:'dockerhubID'
        )
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("${IMAGE_NAME}:latest")
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/','dockerhubID') {
            dockerImage.push()
          }
        }
      }
    }

    post {
      always {
        echo "Cleaning up workspacce..."
        deleteDir()
      }
      success {
        echo 'Pipeline succeeded!'
      }
      failure {
        echo 'Pipeline failed!'
      }
    }
}
  
