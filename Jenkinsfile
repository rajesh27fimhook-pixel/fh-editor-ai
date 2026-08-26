pipeline {
    agent any

    environment {
       IMAGE_NAME = 'ghcr.io/rajesh27fimhook-pixel/fh-editor-ai'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Check') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:ci .'
            }
        }

        stage('Test Docker Image') {
            steps {
                sh 'docker run --rm ${IMAGE_NAME}:ci python -c "import torch, cv2, numpy, scipy, skimage; print(\\"AI environment OK\\")"'
            }
        }

        stage('Login to GHCR') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'ghcr-editor-ai',
                        usernameVariable: 'GHCR_USER',
                        passwordVariable: 'GHCR_TOKEN'
                    )
                ]) {
                    sh 'echo "$GHCR_TOKEN" | docker login ghcr.io -u "$GHCR_USER" --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push ${IMAGE_NAME}:ci'
            }
        }
    }

    post {
        always {
            sh 'docker logout ghcr.io || true'
        }
    }
}