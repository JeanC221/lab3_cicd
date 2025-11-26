pipeline {
    agent any
    
    environment {
        PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "${env.BRANCH_NAME == 'main' ? 'nodemain' : 'nodedev'}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:v1.0 ."
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    sh """
                        docker stop ${IMAGE_NAME}-container || true
                        docker rm ${IMAGE_NAME}-container || true
                        docker run -d --name ${IMAGE_NAME}-container -p ${PORT}:3000 ${IMAGE_NAME}:v1.0
                    """
                }
            }
        }
    }
}