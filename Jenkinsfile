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

    stage('Port Forward & Show URL') {
            steps {
                script {
                    // Find a free local port (5000 in our case)
                    sh """
                    nohup kubectl port-forward service/flask-service 5000:80 > /dev/null 2>&1 &
                    sleep 3
                    echo "✅ Application is live at: http://127.0.0.1:5000"
                    """
                }
            }
        }
    

    post {
        success {
            echo "🚀 Deployment successful! Access your app at: http://127.0.0.1:5000"
        }
        failure {
            echo "❌ Build or deployment failed!"
        }
    }

}
