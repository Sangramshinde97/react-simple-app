pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Package') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

         stage('Build and Push Docker Image') {
      agent any
      environment {
        AWS_REGION = 'ap-south-1'
        ECR_REPOSITORY_NAME = '433796583082.dkr.ecr.ap-south-1.amazonaws.com/dockerimage'
        DOCKER_IMAGE_NAME = "dockerimage"
        DOCKERFILE_PATH = "." // e.g., "java-maven-sonar-argocd-helm-k8s/spring-boot-app/Dockerfile"
      }
      steps {
        script {
          // Build the Docker image
          sh " sudo docker build -t $DOCKER_IMAGE_NAME $DOCKERFILE_PATH"

          // Log in to AWS ECR
          sh "aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPOSITORY_NAME"

          // Tag the Docker image with the ECR repository URI
          def ecrImageURI = "$ECR_REPOSITORY_NAME:${BUILD_NUMBER}"
          sh "docker tag $DOCKER_IMAGE_NAME $ecrImageURI"

          // Push the image to ECR
          sh "docker push $ecrImageURI"
        }
      }
    }
    }
}

