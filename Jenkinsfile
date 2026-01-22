pipeline {
  agent any

  environment {
    DOCKER_USER = "supriya242"
  }

  stages {
    stage('Build App1 Image') {
      steps {
        sh 'docker build -t $DOCKER_USER/app1:latest app1/'
        sh 'docker push $DOCKER_USER/app1:latest'
      }
    }

    stage('Build App2 Image') {
      steps {
        sh 'docker build -t $DOCKER_USER/app2:latest app2/'
        sh 'docker push $DOCKER_USER/app2:latest'
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
          kubectl apply -f k8s/
          kubectl rollout status deployment/app1-deployment
          kubectl rollout status deployment/app2-deployment
        '''
      }
    }
  }
}


