pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Meghasss/aws-devops-demo-project.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t megha-app:v1 .'
            }
        }
        stage('Deploy Container') {
            steps {
                sh 'docker rm -f megha-app || true'
                sh 'docker run -d -p 8082:80 --name megha-app megha-app:v1'
            }
        }
    }
}
