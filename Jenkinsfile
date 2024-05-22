pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        checkout scmGit(branches: [[name: '*/main']], 
                        extensions: [], 
                        userRemoteConfigs: [[ url: 'https://github.com/Ambika-cloudside/jenkins-repo.git']])
      }
    }
    stage('Build Docker Image') {
      steps {
        script {
          // Build the image (modified name within 'withRegistry' block)
            dockerImage = docker.build("ambikadutt/flask-repo", "-f ./Dockerfile .")
          }
        }
      }
    
    
        }
      }
    
