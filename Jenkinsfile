pipeline {
    agent any
    environment {
        DOCKERHUB_USER = "ksuresh58"
        DOCKER_IMAGE  = "exp8app"
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
                    // login safely using password-stdin
                    bat """
                    echo %PASS% | docker login -u %USER% --password-stdin
                    docker push %DOCKERHUB_USER%/%DOCKER_IMAGE%:latest
                    """
                }
            }
        }
    
stage('Deploy to Kubernetes') {
    steps {
        withCredentials([file(credentialsId: 'docker-desktop-kubeconfig', variable: 'KUBECONFIG_FILE')]) {
            bat 'kubectl --kubeconfig=%KUBECONFIG_FILE% apply -f myapp-deployment.yaml'
            bat 'kubectl --kubeconfig=%KUBECONFIG_FILE% apply -f myapp-service.yaml'
            //bat 'kubectl rollout restart deployment app'
        }
    }
}

}        
       
  
}
