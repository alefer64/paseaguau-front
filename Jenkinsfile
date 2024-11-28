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

    stage('Update Manifests') {
    steps {
        script {
            sh '''
            sed -i 's|image: localhost:5000/paseaguau:.*|image: localhost:5000/paseaguau:${BUILD_NUMBER}|' deployment/deployment/paseaguau-ui-deployment.yaml
            git add deployment/deployment/paseaguau-ui-deployment.yaml
            git commit -m "Update image to paseaguau:${BUILD_NUMBER}"
            git push origin production
            '''
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
