pipeline {
  agent any
   environment {
        PROJECT_ID = 'cloudside-academy'
        ARTIFACT_REGISTRY_REGION = 'us-central1'  
        REPOSITORY_NAME = 'ambika-repo'
        APP_NAME = 'helloworld' 
        IMAGE_NAME = 'us-central1-docker.pkg.dev/cloudside-academy/ambika-repo/helloworld'
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
           docker.build("${APP_NAME}", "-f ./Dockerfile .")
          }
        }
      }
    stage('Push Docker Image to Artifact Registry') {
            steps {
                script {
                    withCredentials([file(credentialsId: "f1f2aef4-3297-4de3-a439-83def74f0a40", variable: 'GCR_CRED')]){
                             sh 'gcloud auth activate-service-account --key-file=${GCR_CRED}'
                              sh 'gcloud auth configure-docker us-central1-docker.pkg.dev/cloudside-academy/ambika-repo--quiet'
                              sh "docker tag ${APP_NAME}:${IMAGE_TAG} ${IMAGE_NAME}"
                              sh 'docker push ${IMAGE_NAME}'  
                    }
          }

              
        }
      }
      stage('Gke deploy') {
            steps {
                script {
                  withCredentials([file(credentialsId: "f1f2aef4-3297-4de3-a439-83def74f0a40", variable: 'GCR_CRED')]){
                    sh 'gcloud auth activate-service-account --key-file=${GCR_CRED}'
                      sh 'gcloud config set project cloudside-academy'
                   sh 'gcloud deploy apply --file=clouddeploy.yaml --region=us-central1-a --project=cloudside-academy '
                    sh 'gcloud deploy releases create gke-nodeapp-release-$SHORT_SHA \
                           --project=cloudside-academy \
                            --region=us-central1-a \
                            --delivery-pipeline= my-gke-demo-app-1\
                            --images= us-central1-docker.pkg.dev/cloudside-academy/ambika-repo/helloworld'
                  
                  }    
            }
            }
  }
  } 
}
