
# Kubernetes Demo App

This repository demonstrates deploying and managing a **containerized Spring Boot application** on Kubernetes using **k3s**. It covers core Kubernetes objects, scaling, rolling updates, rollback and troubleshooting.

----------

## Prerequisites
- Linux VM or EC2 instance (Amazon Linux 2 recommended)  
- If it is an EC2 instance, open port  30007 in security group 

---

## Repository Structure

```
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
├── screenshot/
├── README.md
└── deployment-logs.txt
```

----------

## Installation and setup steps

**1. Install k3s**

```bash
curl -sfL https://get.k3s.io | sh -
```

**2. Verify cluster**

```bash
sudo kubectl get nodes
```

**3. Allow kubectl without sudo**

```bash
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $(id -u):$(id -g) ~/.kube/config
export KUBECONFIG=~/.kube/config
```

**4. Verify**

```bash
kubectl cluster-info
kubectl get pods -A
```

## Kubernetes Manifests

Download configuration files

```bash
git clone https://github.com/tehreem-k/SecDevOps-Assignment-Kubernetes.git
cd SecDevOps-Assignment-Kubernetes/k8s
```

## Deployment Steps

```bash
kubectl apply -f .

```

## Verify Resources

```bash
kubectl get pods
kubectl get svc
kubectl get deploy

```

## Scale Application

```bash
kubectl scale deployment demo-app --replicas=5

```

## Access Application

```bash
curl http://<EC2-PUBLIC-IP>:30007

```

## Rolling Update
Deploying a new version

```bash
kubectl set image deployment/demo-app demo=tehreemgmail/secdevops-assignment-springboot-basic:2.0
kubectl rollout status deployment/demo-app
kubectl rollout history deployment/demo-app

```

## Access Application
This time you will get an updated version

```bash
curl http://<EC2-PUBLIC-IP>:30007

```

## Rollback

```bash
kubectl rollout undo deployment/demo-app

```

## Access Application
You will get previous version os app

```bash
curl http://<EC2-PUBLIC-IP>:30007

```
----------

## Issue: ImagePullBackOff

### Problem

Pods failed to start due to incorrect Docker image name.

### Symptoms

```bash
kubectl get pods

```

Status showed: `ImagePullBackOff`

### Debugging Steps

```bash
kubectl describe pod <pod-name>

```

Error message:

```
Failed to pull image "tehreemgmail/secdevops-assignment-springboot-basic:latest"

```

### Root Cause

Incorrect Docker image name in deployment manifest.

### Resolution

-   Corrected image name
    
-   Re-applied deployment
    

```bash
kubectl apply -f deployment.yaml

```

### Result

Pods moved to `Running` state and application became accessible.

----------

