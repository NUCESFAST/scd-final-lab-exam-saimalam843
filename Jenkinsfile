pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the repository
                git 'https://github.com/your/repository.git'
            }
        }
        
        stage('Build') {
            steps {
                // Build your Docker images
                sh 'docker build -t auth-service:latest -f Dockerfile.auth .'
                sh 'docker build -t classrooms-service:latest -f Dockerfile.classrooms .'
                sh 'docker build -t post-service:latest -f Dockerfile.post .'
            }
        }

        stage('Push Docker Images') {
            steps {
                // Push Docker images to a Docker registry
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                }
                sh 'docker push auth-service:latest'
                sh 'docker push classrooms-service:latest'
                sh 'docker push post-service:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Apply Kubernetes manifests
                sh 'kubectl apply -f k8s/auth-deployment.yml'
                sh 'kubectl apply -f k8s/auth-service.yml'
                sh 'kubectl apply -f k8s/classrooms-deployment.yml'
                sh 'kubectl apply -f k8s/classrooms-service.yml'
                sh 'kubectl apply -f k8s/post-deployment.yml'
                sh 'kubectl apply -f k8s/post-service.yml'
            }
        }
    }
}
