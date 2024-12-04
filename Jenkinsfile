pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "hello-world-app:latest"
        CONTAINER_NAME = "hello-world-container"
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
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove the container if it already exists
                    bat """
                    docker ps -q --filter "name=${CONTAINER_NAME}" | grep -q . && docker stop ${CONTAINER_NAME} && docker rm ${CONTAINER_NAME} || true
                    docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Application deployed successfully in Docker!'
        }
        failure {
            echo 'Deployment failed. Check the logs for details.'
        }
    }
}
