# Pull Docker Images from ACR using Service Principal and Run on Azure Virtual Nodes
## Step-01: Introduction
- We are going to pull Images from Azure Container Registry which is not attached to AKS Cluster.
- We are going to do that using Azure Service Principals.
- Build a Docker Image from our Local Docker on our Desktop
- Push to Azure Container Registry
- Create Service Principal and using that create Kubernetes Secret.
- Using Kubernetes Secret associated to Pod Specification, pull the docker image from Azure Container Registry and Schedule on Azure AKS Virtual Nodes

![Image](https://stacksimplify.com/course-images/azure-kubernetes-service-and-acr-virtualnodes.png "Azure AKS Kubernetes - Masterclass")

## Step-02: Build Docker Image Locally