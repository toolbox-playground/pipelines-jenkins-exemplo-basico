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

    stage('Build') {
      steps {
        def customImage = docker.build("my-image:${env.BUILD_ID}")
        customImage.push()
        customImage.push('latest')
      }
    }
  }
}