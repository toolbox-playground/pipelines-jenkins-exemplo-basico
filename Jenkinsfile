pipeline {
    agent {
        docker {
            image 'node:alpine'
            args '-v /var/run/docker.sock:/var/run/docker.sock' // Mount the Docker socket
        }
    }
    environment {
        // Define environment variables
        DOCKER_IMAGE_NAME = 'your-docker-image-name'
        DOCKER_TAG = 'latest'
        DOCKER_REPO = 'toolboxplayground/jenkins'
    }
    stages {
        stage('Install') {
            steps {
                sh 'cd app && npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'cd app && npm test'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh """
                    docker build -t ${env.DOCKER_IMAGE_NAME}:${env.DOCKER_TAG} .
                    """
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Inject Docker Hub credentials into environment variables
                    withCredentials([usernamePassword(credentialsId: 'marcelobuzzettidocker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Log in to Docker Hub and push the Docker image
                        sh """
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        docker push ${env.DOCKER_REPO}/${env.DOCKER_IMAGE_NAME}:${env.DOCKER_TAG}
                        docker logout
                        """
                    }
                }
            }
        }
    }
    post {
        always {
            // Clean up the Docker environment
            sh 'docker system prune -af'
        }
    }
}
