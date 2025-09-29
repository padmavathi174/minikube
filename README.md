# Kubernetes Task 5 - Minikube Cluster Setup (Windows)

This repository contains a complete, Windows-optimized, phased guide to set up a local Kubernetes cluster using Minikube, deploy an Nginx application, expose it, scale it, perform rolling updates, and organize deliverables.

## Prerequisites
- Docker Desktop for Windows
- Minikube
- kubectl

## Files Included
- `deployment.yaml` — Nginx Deployment
- `service.yaml` — NodePort Service (external access)
- `service-clusterip.yaml` — ClusterIP Service (internal access)
- `configmap.yaml` — ConfigMap example
- `screenshots/` — Place all required screenshots here

---

## Phase 1: Environment Setup (Windows)

Install Docker Desktop from `https://www.docker.com/products/docker-desktop/`, then:
```powershell
docker --version
docker info
```

Install Chocolatey, kubectl, Minikube:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

choco install kubernetes-cli -y --no-progress
choco install minikube -y --no-progress

kubectl version --client
minikube version
```

Start cluster:
```powershell
minikube start --driver=docker
minikube status
kubectl cluster-info
kubectl get nodes
```

---

## Phase 2: Deploy Nginx
```powershell
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get pods
kubectl get pods -o wide
```

---

## Phase 3: Expose Service
```powershell
kubectl apply -f service.yaml
kubectl get services
minikube service my-app-service --url
```
Open the printed URL in your browser to see Nginx.

---

## Phase 4: Scale, Inspect, Debug
```powershell
kubectl scale deployment my-app-deployment --replicas=5
kubectl get pods

kubectl scale deployment my-app-deployment --replicas=2
kubectl get pods

kubectl describe deployment my-app-deployment
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

---

## Phase 5: Advanced (Optional)
```powershell
kubectl create namespace dev-environment
kubectl get namespaces

kubectl apply -f configmap.yaml
kubectl get configmaps
kubectl describe configmap app-config

kubectl apply -f service-clusterip.yaml
kubectl get services
```

Rolling update (edit image in `deployment.yaml`, then):
```powershell
kubectl apply -f deployment.yaml
kubectl rollout status deployment my-app-deployment
kubectl rollout history deployment my-app-deployment
```

---

## Phase 6: Cleanup
```powershell
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml
kubectl delete -f configmap.yaml
minikube stop
# minikube delete
```

---

## Screenshot Checklist (save in `screenshots/`)
- 01-cluster-status.png: `minikube status`, `kubectl cluster-info`, `kubectl get nodes`
- 02-pods-running.png: `kubectl get deployments`, `kubectl get pods`
- 03-service-running.png: `kubectl get services`
- 06-browser-access.png: Nginx welcome page (from `minikube service my-app-service --url`)
- 04-scaled-pods.png: After scaling to 5 replicas, `kubectl get pods`
- 05-describe-output.png: `kubectl describe deployment my-app-deployment`