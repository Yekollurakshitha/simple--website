pipeline {
  agent any
  environment {
    IMAGE = 'yourdockerhubuser/simple-website'
  }
  stages {
    stage('Checkout') {
      steps { git branch: 'main', url: 'https://github.com/youruser/simple-website.git' }
    }
    stage('Build Image') {
      steps { script { sh 'docker build -t $IMAGE:${env.BUILD_NUMBER} .' } }
    }
    stage('Push Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh 'echo $PASS | docker login -u $USER --password-stdin'
          sh 'docker push $IMAGE:${env.BUILD_NUMBER}'
          sh 'docker tag $IMAGE:${env.BUILD_NUMBER} $IMAGE:latest'
          sh 'docker push $IMAGE:latest'
        }
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying via Docker Compose on target server...'
        // Example: sh 'ssh user@server "cd /path/simple-website && docker-compose pull && docker-compose up -d"'
      }
    }
  }
}