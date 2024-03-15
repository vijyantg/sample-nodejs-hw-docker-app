pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="828241315255"
        AWS_DEFAULT_REGION="us-east-1"
        IMAGE_REPO_NAME="nodejsapp"
        IMAGE_TAG="v1"
        REPOSITORY_URI = "public.ecr.aws/z7h8o8r8/nodejsapp"
    }

    stages {
        
         stage('Logging into AWS ECR') {
            steps {
                script {
                sh "aws ecr-public get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin public.ecr.aws/z7h8o8r8"
                }
                 
            }
        }
        
        stage('Cloning Git') {
            steps {
                cleanWs()
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/vijyantg/sample-nodejs-hw-docker-app.git']]])     
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      }
    }
//     {
//     stage('Archive') {
//         archiveArtifacts "*"
//     }
// }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                sh "docker push ${REPOSITORY_URI}:${IMAGE_TAG}"
         }
        }
      }
    }
}
