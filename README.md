# Kubernetes Ingress Multi-Service Project

This project demonstrates deploying multiple applications on Kubernetes and routing traffic using Ingress.

## Tools
- Docker
- Kubernetes
- Jenkins
- NGINX Ingress

## How it works
/app1 → App1 Service → App1 Pods  
/app2 → App2 Service → App2 Pods

## Run on Minikube
minikube start  
minikube addons enable ingress  
kubectl apply -f k8s/  

Add host entry:  
<minikube-ip> myapp.local

