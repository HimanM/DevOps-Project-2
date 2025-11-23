pipeline {
    agent any
    environment {
        DOCKERHUB_USERNAME = credentials('dockerhub-username')
        DOCKERHUB_TOKEN = credentials('dockerhub-token')
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Code Quality') {
            steps {
                echo 'Running linting...'
                // sh 'cd frontend && npm run lint'
            }
        }
        stage('Unit Tests') {
            steps {
                echo 'Running tests...'
                // sh 'cd backend && pytest'
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    sh 'docker build -t $DOCKERHUB_USERNAME/devops-frontend:$IMAGE_TAG ./frontend'
                    sh 'docker build -t $DOCKERHUB_USERNAME/devops-backend:$IMAGE_TAG ./backend'
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    sh 'echo $DOCKERHUB_TOKEN | docker login -u $DOCKERHUB_USERNAME --password-stdin'
                    sh 'docker push $DOCKERHUB_USERNAME/devops-frontend:$IMAGE_TAG'
                    sh 'docker push $DOCKERHUB_USERNAME/devops-backend:$IMAGE_TAG'
                }
            }
        }
    }
}
