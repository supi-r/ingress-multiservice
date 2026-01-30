pipeline {
    agent any

    environment {
        DOCKER_IMAGE_APP1 = "supriya242/app1:latest"
        DOCKER_IMAGE_APP2 = "supriya242/app2:latest"
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
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
                  docker build -t $DOCKER_IMAGE_APP1 app1/
                  docker build -t $DOCKER_IMAGE_APP2 app2/
                '''
            }
        }

        stage('Push Docker Images') {
            steps {
                sh '''
                  docker push $DOCKER_IMAGE_APP1
                  docker push $DOCKER_IMAGE_APP2
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                  echo "Using kubeconfig: $KUBECONFIG"
                  kubectl version --client
                  kubectl get nodes
                  kubectl apply -f k8s/
                  kubectl rollout status deployment/app1
                  kubectl rollout status deployment/app2
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
        always {
            sh 'docker logout'
        }
    }
}

