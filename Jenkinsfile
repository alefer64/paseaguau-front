pipeline {
  agent {
    docker { image 'node:22.11.0-alpine3.20' }
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
                    sh "docker build . paseaguau:${BUILD_NUMBER}"
                }
            }
        }
        
    }
} 
