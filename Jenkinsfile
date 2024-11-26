pipeline {
  agent {
        docker { 
            image 'node:12.16.2'
            args '-p 3000:3000'
        }
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
