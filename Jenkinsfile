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
          
          # Mostrar contenido del archivo antes del cambio
          echo "Contenido original:"
          cat deployment/paseaguau-ui-deployment.yaml
          
          # Actualizar la imagen en el manifiesto de deployment
          sed -i 's|image: localhost:5000/paseaguau:.*|image: localhost:5000/paseaguau:${BUILD_NUMBER}|' deployment/paseaguau-ui-deployment.yaml
          
          # Mostrar contenido del archivo después del cambio
          echo "Contenido después de sed:"
          cat deployment/paseaguau-ui-deployment.yaml
          
          # Verificar si hubo cambios
          git diff --exit-code > /dev/null
          if [ $? -eq 0 ]; then
            echo "No changes detected, skipping commit."
          else
            git add deployment/paseaguau-ui-deployment.yaml
            git diff --exit-code > /dev/null || git add deployment/paseaguau-ui-deployment.yaml && git commit -m "Update image to paseaguau:${BUILD_NUMBER}"
            git push https://${GIT_CREDENTIALS_USR}:${GIT_CREDENTIALS_PSW}@github.com/alefer64/paseaguau-front.git production
          fi
          '''
        }
      }
    }
  }

  post {
    success {
      echo 'Pipeline completado exitosamente.'
    }
    failure {
      echo 'Error en el pipeline.'
    }
  }
}
