pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jeicho123/shop-frontend'
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run lint'
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Security Scan') {
            steps {
                sh 'npm audit --audit-level=high || true'
            }
        }
        stage('Build image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest -t ${IMAGE_NAME}:${GIT_COMMIT} ."
            }
        }
        stage('Push to dockerhub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${IMAGE_NAME}:latest
                        docker push ${IMAGE_NAME}:${GIT_COMMIT}
                        docker logout
                    '''
                }
            }
        }
    }
}
