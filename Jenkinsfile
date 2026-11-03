
pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '1902-8548-9334'
        ECR_REPO = 'devops_project_prac'
        IMAGE_NAME = 'springboot_project'
        ECR_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}"
    }

    stages {

        // Stage 1: Checkout source code
        stage('Checkout') {
            steps {
                echo 'Checking out source code'
                checkout scm
            }
        }

        // Stage 2: Build Java application
        stage('Maven Build') {
            steps {
                echo 'Building Java application'
                sh 'mvn clean package -DskipTests'
            }
        }

        // Stage 3: Build Docker image
        stage('Docker Build') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t my-java-app .'
            }
        }

        // Stage 4: Login to ECR
        stage('ECR Login') {
            steps {
                sh '''
                    aws ecr get-login-password --region $AWS_REGION |
                    docker login --username AWS --password-stdin \
                    ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                '''
            }
        }

        // Stage 5: Push image to ECR
        stage('Push to ECR') {
            steps {
                sh '''
                    docker tag $IMAGE_NAME:latest $ECR_URI:latest
                    docker push $ECR_URI:latest
                '''
            }
        }
    }
}

