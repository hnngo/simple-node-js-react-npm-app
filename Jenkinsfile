pipeline {
  agent {
    docker {
      label 'docker'
      image 'node:alpine'
      args '-p 3000:3000'
    }
  }

  environment {
    CI = 'true'
    registry = "hnngo/jenkins-docker-nodejs"
    registryCredential = 'dockerhub'
  }
  
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh './jenkins/scripts/test.sh'
      }
    }

    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
        input message: 'Finished using the web site? (Click "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
      }
    }

    stage('Docker Publish Image') {
      steps {
        script {
          docker.withRegistry( '', registryCredential ) {
            def image = docker.build registry + ":$BUILD_NUMBER"
            image.push()
          }
        }
      }
    }
  }
}