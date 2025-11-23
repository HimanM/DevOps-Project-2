pipeline {
    agent any
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_CRED_USR/devops-frontend:$IMAGE_TAG ./frontend'
                    sh 'docker build -t $DOCKER_CRED_USR/devops-backend:$IMAGE_TAG ./backend'
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    sh 'echo $DOCKER_CRED_PSW | docker login -u $DOCKER_CRED_USR --password-stdin'
                    sh 'docker push $DOCKER_CRED_USR/devops-frontend:$IMAGE_TAG'
                    sh 'docker push $DOCKER_CRED_USR/devops-backend:$IMAGE_TAG'
                }
            }
        }
    }
}
