pipeline {
  agent any
   environment {
        PROJECT_ID = 'cloudside-project'
        ARTIFACT_REGISTRY_REGION = 'us-central1'  
        REPOSITORY_NAME = 'ambika-repo'
        APP_NAME = 'HELLOWORLD' 
        IMAGE_NAME = 'us-central1-docker.pkg.dev/cloudside-project/${REPOSITORY_NAME}/${APP_NAME}'
        IMAGE_TAG = 'latest'
        
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
           sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} :'
          }
        }
      }
    stage('Push Docker Image to Artifact Registry') {
            steps {
                script {
                    withCredentials([file(credentialsId: "bcf68493-4735-496d-b987-e9e60c5c3ded", variable: 'GCR_CRED')]){
                              sh 'cat "${GCR_CRED}" | docker login -u _json_key_base64 --password-stdin https://"us-central1-docker.pkg.dev'
                                docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    }
          }
    
        }
      }
            }
    
  }
  

