pipeline {
  agent any
   environment {
        PROJECT_ID = 'cloudside-project'
        ARTIFACT_REGISTRY_REGION = 'us-central1'  
        REPOSITORY_NAME = 'ambika-repo'
        IMAGE_NAME = 'ambikadutt/flask-repo'
        }
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
    stage('Push Docker Image to Artifact Registry') {
            steps {
                script {
                    docker.withRegistry("https://${ARTIFACT_REGISTRY_REGION}-docker.pkg.dev", "gcr:us:artifact-registry") {
                        dockerImage.push("latest")
    
        }
      }
            }
    
  }
  }
}
