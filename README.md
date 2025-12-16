
# Kubernetes Demo App

This repository demonstrates deploying and managing a **containerized Spring Boot application** on Kubernetes using **k3s**. It covers core Kubernetes objects, scaling, rolling updates, rollback and troubleshooting.

----------

## Prerequisites
- Linux VM or EC2 instance (Amazon Linus 2 recommended)  
- If it is an EC2 instance, open port  30007 in security group 

---

## Repository Structure

```
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
├── cluster-setup.md
├── README.md
├── screenshot/
└── deployment-logs.txt
```

----------

## Build docker image and push on dockerhub Steps
**1. Update and install docker**
```bash
git clone https://github.com/tehreem-k/SecDevOps-Assignment-Docker.git
cd SecDevOps-Assignment-Docker
```

## Deployment Steps


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

## Rolling Update

```bash
kubectl set image deployment/demo-app demo=<dockerhub-username>/demo-app:2.0
kubectl rollout status deployment/demo-app
kubectl rollout history deployment/demo-app

```

## Rollback

```bash
kubectl rollout undo deployment/demo-app

```

## Access Application

```bash
curl http://<EC2-PUBLIC-IP>:30007

```

----------

# 🛠️ cluster-setup.md

## Kubernetes Cluster Setup (k3s on Amazon Linux 2)

### Install Docker

```bash
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user

```

### Install k3s

```bash
curl -sfL https://get.k3s.io | sh -

```

### Configure kubectl

```bash
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $(id -u):$(id -g) ~/.kube/config
export KUBECONFIG=~/.kube/config

```

### Verify Cluster

```bash
kubectl get nodes
kubectl cluster-info

```

----------

# 📄 deployment-logs.txt (Sample)

```
$ kubectl get pods
NAME                        READY   STATUS    AGE
...

$ kubectl get svc
NAME           TYPE       CLUSTER-IP    PORT(S)
...

$ kubectl rollout status deployment/demo-app
successfully rolled out

```

----------

# 🐞 debug-report.md

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
Failed to pull image "wronguser/demo-app:1.0"

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

✅ This submission demonstrates Kubernetes deployment, scaling, configuration, rollout/rollback, and troubleshooting following best practices.

