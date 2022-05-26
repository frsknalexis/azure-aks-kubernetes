# Azure AKS Pull Docker Images from ACR using Service Principal

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

## Step-01: Introduction
- We are going to pull Images from Azure Container Registry which is not attached to AKS Cluster.
- We are going to do that using Azure Service Principals.
- Build a Docker Image from our Local Docker on our Desktop
- Push to Azure Container Registry
- Create Service Principal and using that create Kubernetes Secret.
- Using Kubernetes Secret associated to Pod Specificaiton, pull the docker image from Azure Container Registry and Schedule on Azure AKS NodePools

![Image](https://stacksimplify.com/course-images/azure-kubernetes-service-and-acr-nodepools.png "Azure AKS Kubernetes - Masterclass")

## Step-02: Create Azure Container Registry
- Go to Services -> Container Registries
- Click on **Add**
- Subscription: Suscripción de Azure 1
- Resource Group: multi-aks-rg-2
- Registry Name: acrforakscluster2 (NAME should be unique across Azure Cloud)
- Location: Central US
- SKU: Basic  (Pricing Note: $0.167 per day)
- Click on **Review + Create**
- Click on **Create**

## Step-03: Build Docker Image Locally

## Step-04: Run locally and test

## Step-05: Enable Docker Login for ACR Repository

## Step-06: Push Docker Image to Azure Container Registry

### Build, Test Locally, Tag and Push to ACR

### Verify Docker Image in ACR Repository
- Go to Services -> Container Registries -> acrforakscluster2
- Go to **Repositories** -> **app2/kube-nginx-acr-app2**

## Step-07: Create Service Principal to access Azure Container Registry
- Update ACR_NAME with your container registry name
- Update SERVICE_PRINCIPAL_NAME as desired
```sh
#!/bin/bash
# This script requires Azure CLI version 2.25.0 or later. Check version with `az --version`.

# Modify for your environment.
# ACR_NAME: The name of your Azure Container Registry
# SERVICE_PRINCIPAL_NAME: Must be unique within your AD tenant
ACR_NAME=acrforakscluster2
SERVICE_PRINCIPAL_NAME=acr-aks-sp

# Obtain the full registry ID for subsequent command args
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create the service principal with rights scoped to the registry.
# Default permissions are for docker pull access. Modify the '--role'
# argument value as desired:
# acrpull:     pull only
# acrpush:     push and pull
# owner:       push, pull, and assign roles
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --scopes $ACR_REGISTRY_ID --role acrpull --query password --output tsv)
SP_APP_ID=$(az ad sp list --display-name $SERVICE_PRINCIPAL_NAME --query [].appId --output tsv)

# Output the service principal's credentials; use these in your services and
# applications to authenticate to the container registry.
echo "Service principal ID: $SP_APP_ID"
echo "Service principal password: $SP_PASSWD"
```

## Step-08: Create Image Pull Secret
```
# Template
kubectl create secret docker-registry <secret-name> \
    --namespace <namespace> \
    --docker-server=<container-registry-name>.azurecr.io \
    --docker-username=<service-principal-ID> \
    --docker-password=<service-principal-password>

# Replace
kubectl create secret docker-registry acr-aks-secret \
    --namespace default \
    --docker-server=acrforakscluster2.azurecr.io \
    --docker-username=2f00f72e-b0d9-415e-a522-8c3e33daad28 \
    --docker-password=TiVAlOXyISlZoZOrFh_CaK6GG27k_TqLeY

# List Secrets
kubectl get secrets
```
## Step-09: Review, Update & Deploy to AKS & Test