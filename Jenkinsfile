pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-hello'
    }

    stages {
        stage('Code Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build+Tag') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME}:latest .
                    docker tag ${IMAGE_NAME}:latest ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }
    }
}
