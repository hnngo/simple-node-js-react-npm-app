pipeline {
  agent {
    label 'docker'
  }

  environment {
    CI = 'true'
    registry = "hnngo/jenkins-docker-nodejs"
    registryCredential = 'dockerhub'
  }
  
  stages {
    stage('Build') {
      agent {
        docker {
          image 'node:alpine'
        }
      }
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      agent {
        docker {
          image 'node:alpine'
        }
      }
      steps {
        sh './jenkins/scripts/test.sh'
      }
    }

    stage('Deliver') {
      agent {
        docker {
          image 'node:alpine'
        }
      }
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