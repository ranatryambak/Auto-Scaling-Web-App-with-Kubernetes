pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "auto-scaling-flask-app"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/ranatryambak/Auto-Scaling-Web-App-with-Kubernetes'
            }
        }

        stage('Point Docker to Minikube') {
            steps {
                sh 'eval $(minikube docker-env)'
            }
        }

        stage('Build Docker Image in Minikube') {
            steps {
                sh """
                docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                """
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh """
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                kubectl apply -f hpa.yaml
                """
            }
        }

    }

    post {
        success {
            echo "🚀 Deployment successful without Docker Hub!"
        }
        failure {
            echo "❌ Build or deployment failed!"
        }
    }
}
