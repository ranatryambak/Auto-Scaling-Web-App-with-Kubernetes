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

