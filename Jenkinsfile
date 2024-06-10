pipeline {
  agent {
    docker {
      image 'node:alpine'
    }

  }
  node {
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
      steps{
        nodejs = docker.build("my-image:${env.BUILD_ID}")
      }
      // steps {
      //   docker.build("my-image:${env.BUILD_ID}")
      //   // customImage.push()
      //   // customImage.push('latest')
      // }
    }
  }
}