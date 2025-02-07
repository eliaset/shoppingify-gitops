# DevOps Engineer Practical Assessment

## Overview
This project simulates a real-world DevOps environment by setting up and managing a Kubernetes-based deployment pipeline with monitoring and CI/CD. The key objectives include:
- Creating a Kubernetes cluster using Kind.
- Installing essential DevOps tools using Helm.
- Deploying a full application stack (frontend, backend, and database).
- Configuring NGINX as a reverse proxy and request mirroring.
- Automating deployments with ArgoCD.
- Monitoring application and infrastructure performance using Prometheus and Grafana.

## Setup Instructions

### 1. Kubernetes Cluster Setup
A Kubernetes cluster with one master node and three worker nodes is created using Kind.

#### Steps:
1. Create the cluster using the following command:
   ```bash
   kind create cluster --config kind-cluster-config.yaml
   ```
2. Verify the cluster setup:
   ```bash
   kubectl get nodes
   ```

### 2. Install Tools Using Helm Charts
Helm is used to install the following tools:
- **ArgoCD**: GitOps-based deployment
- **Prometheus**: Application and infrastructure monitoring
- **Grafana**: Visualization of collected metrics
- **MongoDB**: Database with persistent storage

#### Steps:
1. Add the Helm repositories and install the tools:
   ```bash
   helm repo add argo https://argoproj.github.io/argo-helm
   helm install argocd argo/argo-cd -n argocd --create-namespace
   
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm install prometheus prometheus-community/prometheus -n monitoring --create-namespace
   helm install grafana grafana/grafana -n monitoring
   ```

### 3. Deploy Applications
Applications deployed:
- **Frontend**
- **Backend**
- **MongoDB**

#### Steps:
1. Apply the YAML files for deployments and services:
   ```bash
   kubectl apply -f namespace.yaml,mongodb-deployment.yaml,mongodb-service.yaml,mongodb-pvc.yaml
   kubectl apply -f namespace.yaml,backend-deployment.yaml,backend-service.yaml,frontend-deployment.yaml,frontend-service.yaml
   ```

### 4. Duplicate Backend Application
A duplicate backend instance is deployed in a separate namespace.

#### Steps:
1. Create a new namespace:
   ```bash
   kubectl apply -f duplicate-namespace.yaml
   ```
2. Deploy the duplicate backend:
   ```bash
   kubectl apply -f duplicate-backend-deployment.yaml -f duplicate-backend-service.yaml
   ```

### 5. Setup NGINX as Proxy and Mirror Requests
NGINX is configured as a reverse proxy to forward and mirror backend requests.

#### Steps:
1. Apply the NGINX configuration:
   ```bash
   kubectl apply -f nginx-configmap.yaml
   kubectl apply -f nginx-deployment.yaml -f nginx-service.yaml
   ```

### 6. Configure ArgoCD for Deployment
ArgoCD is set up to automatically deploy applications from a Git repository.

#### Steps:
1. Configure ArgoCD to sync with the Git repository.
2. Verify ArgoCD deployment:
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   ```

### 7. Monitor Applications Using Prometheus and Grafana
Application and infrastructure metrics are collected and visualized using Prometheus and Grafana.

#### Metrics Collected:
- **Application-level**: Response times, error rates
- **Infrastructure-level**: CPU usage, memory consumption

