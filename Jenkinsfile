pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "tryambak79/auto-scaling-flask-app"
        DOCKER_TAG = "latest"
        DOCKER_CREDENTIALS = credentials('docker-hub-creds') // ID from Jenkins credentials
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/ranatryambak/Auto-Scaling-Web-App-with-Kubernetes'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                """
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh """
                echo "$DOCKER_CREDENTIALS_PSW" | docker login -u "$DOCKER_CREDENTIALS_USR" --password-stdin
                docker push $DOCKER_IMAGE:$DOCKER_TAG
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
            echo "✅ Deployment to Minikube successful!"
        }
        failure {
            echo "❌ Build or deployment failed!"
        }
    }
}
