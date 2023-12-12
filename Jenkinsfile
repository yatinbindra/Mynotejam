pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t yatinb47/mynotejam:$(git rev-parse --short HEAD) ./notejam'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push yatinb47/mynotejam:$(git rev-parse --short HEAD)'
      }
    }
    stage('Pull') {
      steps {
        sh 'docker pull yatinb47/mynotejam:$(git rev-parse --short HEAD)'
      }
    }
     stage('Deploy') {
      steps {
        sh '''
            a=$(docker rm -f notejam)
            if [a=="notejam"]
            then
            docker run -dp 9000:3000 --name notejam yatinb47/mynotejam:$(git rev-parse --short HEAD) 
            else
            docker run -dp 9000:3000 --name notejam yatinb47/mynotejam:$(git rev-parse --short HEAD) 
            fi
            '''
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}