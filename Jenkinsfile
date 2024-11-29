pipeline {
  agent any

  tools {
    nodejs '18.18.0'
  }

  environment {
    REGISTRY = 'localhost:5000'
    IMAGE_NAME = 'paseaguau'
    GIT_CREDENTIALS = credentials('my-git-credentials-id')
  }

  stages {
    
    stage('Build Image') {
            when {
                anyOf {
                    changeRequest()
                    branch 'develop'
                    branch 'production'
                }
            }
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
      when {
        branch 'production'
      }
      steps {
        script {
          sh '''
          cd paseaguau-front
          git config user.name "alefer64"
          git config user.email "alejandro.fernandez141@davinci.edu.ar"
          echo ${BUILD_NUMBER}
          sed -i "s|image: localhost:5000/paseaguau:.*|image: localhost:5000/paseaguau:${BUILD_NUMBER}|" deployment/paseaguau-ui-deployment.yaml
          git add deployment/paseaguau-ui-deployment.yaml
          git commit -m "Update image to paseaguau:${BUILD_NUMBER}"
          git push https://${GIT_CREDENTIALS_USR}:${GIT_CREDENTIALS_PSW}@github.com/alefer64/paseaguau-front.git production --no-verify
          '''
        }
      }
    }
  }

  post {
    success {
      cleanWs()
      echo 'Pipeline completado exitosamente.'
    }
    failure {
      cleanWs()
      echo 'Error en el pipeline.'
    }
  }
}
