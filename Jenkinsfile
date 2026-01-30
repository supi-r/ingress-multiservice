pipeline {
    agent any

    environment {
        APP1_IMAGE = "supriya242/app1:latest"
        APP2_IMAGE = "supriya242/app2:latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/supriya242/ingress-multiservice'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                  docker build -t $APP1_IMAGE ./app1
                  docker build -t $APP2_IMAGE ./app2
                '''
            }
        }

        stage('Push Docker Images') {
            steps {
                sh '''
                  docker push $APP1_IMAGE
                  docker push $APP2_IMAGE
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                  kubectl apply -f k8s/
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment completed successfully"
        }
        failure {
            echo "❌ Deployment failed"
        }
    }
}

