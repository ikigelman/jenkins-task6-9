pipeline {
    agent any

    environment {
        IMAGE_NAME = 'iritkigelman/flask-hello'
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

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        set +x
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${IMAGE_NAME}:latest
                        docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                        docker logout
                    '''
                }
            }
        }
    }
}
