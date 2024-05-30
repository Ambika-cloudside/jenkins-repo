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
                    withCredentials([file(credentialsId: "bde0b6a2-15ce-4ed4-bf74-3b543578d9ca", variable: 'GCR_CRED')]){
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
                  withCredentials([file(credentialsId: "bde0b6a2-15ce-4ed4-bf74-3b543578d9ca", variable: 'GCR_CRED')]){
                    sh 'gcloud auth activate-service-account --key-file=${GCR_CRED}'
                      sh 'gcloud config set project cloudside-academy'
                   sh 'gcloud deploy apply --file=clouddeploy.yaml --region=us-central1 --project=cloudside-academy '
                    sh 'gcloud deploy releases create gke-nodeapp-release-$SHORT_SHA \
                           --project=cloudside-academy \
                            --region=us-central1 \
                            --delivery-pipeline=my-gke-demo-app-1\
                            --images=helloworld=us-central1-docker.pkg.dev/cloudside-academy/ambika-repo/helloworld'
                  
                  }    
            }
            }
  }
  } 
}
