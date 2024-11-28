pipeline {
  agent any

    tools {
        nodejs '18.18.0'
    }
  
    stages {
      
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }
        
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
