// Jenkinsfile
pipeline {
    agent any // This means the pipeline can run on any available agent (your Jenkins machine)

    environment {
        // Define common variables
        DOCKER_IMAGE_NAME = "finalvot"
        DOCKER_CONTAINER_NAME = "finalvot-container"
        APP_PORT = "3000"
        HOST_PORT = "8081" // Map container port 3000 to host port 8081
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Jenkins automatically checks out the code when using SCM
                script {
                    echo "Code checked out from SCM"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    // Build the Docker image from the Dockerfile in the current directory
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} ."
                    sh "docker tag ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} ${DOCKER_IMAGE_NAME}:latest"
                    echo "Docker image ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} built successfully."
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    echo "Stopping and removing old container (if any)..."
                    // Stop and remove any old running container
                    sh "docker stop ${DOCKER_CONTAINER_NAME} || true"
                    sh "docker rm ${DOCKER_CONTAINER_NAME} || true"

                    echo "Starting new Docker container..."
                    // Run the new Docker container, mapping ports
                    sh "docker run -d --name ${DOCKER_CONTAINER_NAME} -p ${HOST_PORT}:${APP_PORT} ${DOCKER_IMAGE_NAME}:latest"
                    echo "Application deployed and running on http://localhost:${HOST_PORT}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
        success {
            echo "Pipeline succeeded!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
