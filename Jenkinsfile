pipeline {
  agent {
    docker { image 'node:16-alpine'}
  }
  tools {
  <...>
  'org.jenkinsci.plugins.docker.commons.tools.DockerTool' 'docker'
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
