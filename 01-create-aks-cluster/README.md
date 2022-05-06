# Create AKS Cluster

## Step 01: Introduction
- Understand about AKS Cluster
- Discuss about Kubernetes Architecture from AKS Cluster perspective

## Step 02: Cloud Shell - Configure kubectl to connect to AKS Cluster
```
# Template
az aks get-credentials --resource-group <resource-group-name> --name <cluster-name>

# Replace resource-group-name && cluster-name
az aks get-credentials --resource-group multi-aks-rg --name aks-cluster

# List Kubernetes Worker Nodes
kubectl get nodes
kubectl get nodes -o wide
```

## Step 03: Explore Cluster Control Plane and Workload inside that
```
# List Namespaces
kubectl get namespaces
kubectl get ns

# List Pods from all namespaces
kubectl get pods --all-namespaces

# List all k8s objects from Cluster Control plane
kubectl get all --all-namespaces
```

## Step 04: Local Desktop - Install Azure CLI and Azure AKS CLI
```
# Install Azure CLI (MAC)
brew update && brew install azure-cli

# Login to Azure
az login

# Install Azure AKS CLI
az aks install-cli

# Configure Cluster Creds (kube config)
az aks get-credentials --resource-group multi-aks-rg --name aks-cluster

# List AKS Nodes
kubectl get nodes
kubectl get nodes -o wide
```

## Step 05: Deploy Sample Application and Test
```
# Deploy Application
kubectl apply -f deployment.yaml,service.yaml

# Verify Pods
kubectl get pods

# Verify Deployment
kubectl get deployment

# Verify Service (Make a note of external ip)
kubectl get service
kubectl get svc

# Access Application
http://<external-ip-from-get-service-output>
```