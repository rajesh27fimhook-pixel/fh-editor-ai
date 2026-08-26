pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Environment Check') {
            steps {
                sh 'python --version || true'
                sh 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t editor-ai:ci .'
            }
        }

        stage('Test Docker Image') {
            steps {
                sh 'docker run --rm editor-ai:ci python -c "import torch, cv2, numpy, scipy, skimage; print(\\"AI environment OK\\")"'
            }
        }

    }
}