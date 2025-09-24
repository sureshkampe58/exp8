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
                sh 'docker build -t $DOCKERHUB_USER/$DOCKER_IMAGE:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKERHUB_USER/$DOCKER_IMAGE:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
