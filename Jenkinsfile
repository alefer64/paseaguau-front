pipeline {
  agent any

  tools {
    nodejs '18.18.0'
  }

  environment {
    REGISTRY = 'localhost:5000'
    IMAGE_NAME = 'paseaguau'
  }

  stages {
    stage('Build Image') {
      steps {
        script {
          sh "docker build . -t ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
        }
      }
    }

    stage('Push Image') {
      when {
        branch 'production'
      }
      steps {
        script {
          sh "docker push ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
        }
      }
    }
  }

  post {
    success {
      echo 'Build y push completados exitosamente.'
    }
    failure {
      echo 'Error en el pipeline.'
    }
  }
}
