pipeline {
    agent any
    environment {
        DOCKERHUB_USER = "your-dockerhub-username"
        DOCKER_IMAGE = "exp8-python-app"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sureshkampe58/exp8.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t $DOCKERHUB_USER/$DOCKER_IMAGE:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat 'echo $PASS | docker login -u $USER --password-stdin'
                    bat 'docker push $DOCKERHUB_USER/$DOCKER_IMAGE:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
