pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "auto-scaling-flask-app"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/<your-repo>.git'
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

        stage('Port Forward & Show URL') {
            steps {
                sh """
                pkill -f "kubectl port-forward" || true
                nohup kubectl port-forward service/flask-service 5000:80 > /dev/null 2>&1 &
                sleep 3
                echo "✅ Application is live at: http://127.0.0.1:5000"
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
