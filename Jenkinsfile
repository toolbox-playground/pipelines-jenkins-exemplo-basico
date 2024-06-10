pipeline {
  agent {
    docker {
      image 'node:alpine'
    }

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
  }

  node {
    checkout scm

    // docker.withRegistry('https://docker.io', 'credentials-id') {

        def customImage = docker.build("my-image:${env.BUILD_ID}")

        /* Push the container to the custom Registry */
        // customImage.push()
    // }
  }
}