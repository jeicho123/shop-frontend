pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jeicho123/shop-frontend'
    }

    stages {
        stage('Build image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }
    }
}
