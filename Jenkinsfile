pipeline {
    agent any

    environment {
        IMAGE_NAME = "amritammy/node-app"
        IMAGE_TAG = "latest"
        DOCKER_CREDENTIALS_ID = "docker-hub-credentials"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker rm -f node-app || true"
                    sh "docker run -d -p 80:3000 --name node-app ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
}
