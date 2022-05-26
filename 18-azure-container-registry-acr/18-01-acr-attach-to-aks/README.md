# Integrate Azure Container Registry ACR with AKS

## Step-00: Pre-requisites
- We should have Azure AKS Cluster Up and Running.
- We have created a new aks-cluster-2 cluster as part of Azure Virtual Nodes demo in previous section.
- We are going to leverage the same cluster for all 3 demos planned for Azure Container Registry and AKS.

```
# Configure Command Line Credentials
az aks get-credentials --resource-group multi-aks-rg-2 --name aks-cluster-2

# Verify Nodes
kubectl get nodes
kubectl get nodes -o wide

# Verify aci-connector-linux
kubectl get pods -n kube-system
```

# Step-01: Introduction
- Build a Docker Image from our Local Docker on our Desktop
- Tag the docker image in the required ACR Format
- Push to Azure Container Registry
- Attach ACR with AKS
- Deploy kubernetes workloads and see if the docker image got pulled automatically from ACR we have created.

![Image](https://stacksimplify.com/course-images/azure-kubernetes-service-and-acr.png "Azure AKS Kubernetes - Masterclass")

![Image](https://stacksimplify.com/course-images/azure-container-registry-pricing-tiers.png "Azure AKS Kubernetes - Masterclass")

## Step-02: Create Azure Container Registry
