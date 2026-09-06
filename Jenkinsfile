pipeline {
    agent any
    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO = 'demo'
        ACCOUNT_ID = '448795057644'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Meghasss/aws-devops-demo-project.git'
            }
        }
        stage('Build Jar') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $ECR_REPO:$IMAGE_TAG .'
            }
        }
        stage('Login ECR') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                credentialsId: 'AWS Credentials']])
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin \
                ${ACCOUNT_ID}.dkr.ecr.us-east-1.amazonaws.com/demo
                '''
            }
        }
        stage('Tag Image') {
            steps {
                sh '''
                docker tag $ECR_REPO:$IMAGE_TAG \
                ${ACCOUNT_ID}.dkr.ecr.us-east-1.amazonaws.com/demo/$ECR_REPO:$IMAGE_TAG
                '''
            }
        }
        stage('Push Image') {
            steps {
                sh '''
                docker push \
                $448795057644.dkr.ecr.us-east-1.amazonaws.com/demo/$ECR_REPO:$IMAGE_TAG
                '''
            }
        }
        stage('Deploy EKS') {
            steps {
                sh '''
                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }
}
