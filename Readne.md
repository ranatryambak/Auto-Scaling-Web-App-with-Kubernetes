# Auto-Scaling App on Local Kubernetes (Minikube)

This project demonstrates deploying a containerized application on **Kubernetes** using **Minikube** with a **Horizontal Pod Autoscaler (HPA)**.  
The focus is on **containerization with Docker, Kubernetes deployment, and Jenkins-based automation**.  

---

## 📌 Prerequisites
Before starting, make sure you have:
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed (with Kubernetes disabled, since we’ll use Minikube)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/) installed
- [Kubectl](https://kubernetes.io/docs/tasks/tools/) installed
- [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install) with Ubuntu
- Basic terminal knowledge

---

## 🚀 How to Run This Project (Beginner Friendly)

### 1️⃣ Start Minikube  
This starts your local Kubernetes cluster using Docker.
```bash
minikube start --driver=docker

### 2️⃣ Point Docker to Minikube
eval $(minikube docker-env)


### Build the Docker image:
docker build -t auto-scaling-app:latest .

### Deploy the application to Kubernetes:
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f hpa.yaml

### Check that pods are running:
kubectl get pods

### Access the application locally:
kubectl port-forward service/flask-service 5000:80

### (Optional) Test autoscaling:
### Install Apache Benchmark:
sudo apt install apache2-utils

### Send 1000 requests with 50 concurrent connections:
ab -n 1000 -c 50 http://127.0.0.1:5000/

### Watch autoscaler activity:
kubectl get hpa -w

### Stop everything when done:
minikube stop

### Or delete Minikube completely:
minikube delete --all