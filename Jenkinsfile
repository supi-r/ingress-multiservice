pipeline {
    agent any

    environment {
        DOCKER_APP1_IMAGE = "supriya242/app1:latest"
        DOCKER_APP2_IMAGE = "supriya242/app2:latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/supi-r/ingress-multiservice'
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
                  docker build -t $DOCKER_APP1_IMAGE ./app1
                  docker build -t $DOCKER_APP2_IMAGE ./app2
                '''
            }
        }

        stage('Push Docker Images') {
            steps {
                sh '''
                  docker push $DOCKER_APP1_IMAGE
                  docker push $DOCKER_APP2_IMAGE
                '''
            }
        }


        stage('Deploy to Kubernetes') {
               environment {
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
    }
    steps {
                    sh '''
                      kubectl apply -f k8s/
                      kubectl rollout status deployment/app1
                      kubectl rollout status deployment/app2
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD Pipeline completed successfully"
        }
        failure {
            echo "❌ Pipeline failed. Check logs."
        }
    }
}

