pipeline {
  agent any

    tools {
        nodejs '18.18.0'
    }
  
    stages {
        stage('Build Image') {
            steps {
                script {
                    sh 'docker build . -t localhost:5000/paseaguau:${BUILD_NUMBER}'
                }
            }
        }

        stage('Push Image') {
            steps {
              script {
                  sh 'docker push localhost:5000/paseaguau:${BUILD_NUMBER}'
              }
            }
        }
      
        }
        
}
