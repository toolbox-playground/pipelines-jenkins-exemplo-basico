pipeline {
    agent none
    // agent {
    //     docker {
    //         image 'node:alpine'
    //         args '-v /var/run/docker.sock:/var/run/docker.sock' // Mount the Docker socket
    //     }
    // }
    environment {
        // Define environment variables
        DOCKER_REPO = 'toolboxplayground'
        DOCKER_IMAGE_NAME = 'nodejs-jenkins'
        DOCKER_TAG = 'latest'
        DOCKER_CREDS = credentials('marcelobuzzettidocker')
    }
    stages {
        stage('Install') {
            agent {
                docker {
                    image 'node:alpine'
                }
            }
            steps {
                sh 'cd app && npm install'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:alpine'
                }
            }
            steps {
                sh 'cd app && npm test'
            }
        }
        stage('Build Docker Image') {
            agent any
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${env.DOCKER_REPO}/${env.DOCKER_IMAGE_NAME}:${env.DOCKER_TAG} ."
                }
            }
        }
        stage('Push Docker Image') {
            agent any
            steps {
                script {
                    // Inject Docker Hub credentials into environment variables
                    // withCredentials([usernamePassword(credentialsId: 'marcelobuzzettidocker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Log in to Docker Hub and push the Docker image
                        sh 'echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin'
                        sh "docker push ${env.DOCKER_REPO}/${env.DOCKER_IMAGE_NAME}:${env.DOCKER_TAG}"
                        sh "docker logout"
                    // }
                }
            }
        }
        stage('Pull ant Test Docker Image') {
            agent any
            steps {
                script {
                    // Inject Docker Hub credentials into environment variables
                    // withCredentials([usernamePassword(credentialsId: 'marcelobuzzettidocker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Log in to Docker Hub and push the Docker image
                        sh 'echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin'
                        sh "docker pull ${env.DOCKER_REPO}/${env.DOCKER_IMAGE_NAME}:${env.DOCKER_TAG}"
                        // Select a port to test the application that is not the same as the one used by Jenkins and not the same as the one used by others applications
                        sh "docker run -d -p 8081:8080 --name jenkins-test --rm ${env.DOCKER_REPO}/${env.DOCKER_IMAGE_NAME}:${env.DOCKER_TAG}"
                        sh "sleep 10"
                        sh "docker ps -f 'name=jenkins-test'"
                        sh "docker stop jenkins-test"
                        sh "docker logout"
                    // }
                }
            }
        }
    }
}
