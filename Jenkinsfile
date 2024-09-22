pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'retail-store-website-img'   // Replace with your desired image name
        GIT_REPO_URL = 'https://github.com/waqhasahmed97/retail-store-test.git'  // Replace with your GitHub repo URL
        DOCKER_REGISTRY = 'dockerhub'    // Set this to your Docker registry (optional)
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'  // ID of the Docker Hub credentials stored in Jenkins
    }

    stages {
        stage('Clone Git Repository') {
            steps {
                git branch: 'main', url: "${GIT_REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh """
                    docker build -t ${DOCKER_IMAGE}:latest .
                    """
                }
            }
        }

        stage('Push Docker Image') {
            when {
                expression {
                    return env.DOCKER_REGISTRY != ''  // Only run this step if pushing to DockerHub or a registry is needed
                }
            }
            steps {
                script {
                    // Login to DockerHub or your registry
                    sh """
                    docker login -u ${env.DOCKER_REGISTRY} -p ${env.DOCKER_PASSWORD}
                    docker tag ${DOCKER_IMAGE}:latest ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:latest
                    docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:latest
                    """
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up the local Docker images after build
                sh "docker rmi ${DOCKER_IMAGE}:latest || true"
            }
        }
    }
}
