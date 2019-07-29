node {
  stage('Preparation') {
    checkout scm
  }

  stage('Test') {
    nodejs(nodeJSInstallationName: 'nodejs') {
      sh 'npm install'
      sh 'npm test -- --coverage'
    }
  }

  stage('docker build/push') {
    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
      def app = docker.build("hnngo/jenkins-docker-nodejs", '.').push()
    }
  }
}
