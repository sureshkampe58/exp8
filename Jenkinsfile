pipeline {
    agent any
    environment {
        DOCKERHUB_USER = "ksuresh58"
        DOCKER_IMAGE = "exp8app"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKERHUB_USER%/%DOCKER_IMAGE%:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat 'docker login -u %USER% -p %PASS%'
                    bat 'docker push %DOCKERHUB_USER%/%DOCKER_IMAGE%:v3'
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
