pipeline {
  agent any
  stages {
    stage('Install') {
      steps {
        sh 'cd app && npm install'
      }
    }
    stage('Test'){
      steps{
        sh 'cd app && npm test'
      }
    }
    stage('Build'){
      steps{
        sh 'docker build -t nodejs .'
      }
    }
}