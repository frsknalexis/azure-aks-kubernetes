# Kubernetes Namespaces - Imperative using kubectl

## Step-01: Introduction
- Namespaces allow to split-up resources into different groups.
- Resource names should be unique in a namespace
- We can use namespaces to create multiple environments like dev, staging and production etc
- Kubernetes will always list the resources from `default namespace` unless we provide exclusively from which namespace we need information from.

![Image](https://stacksimplify.com/course-images/azure-kubernetes-service-namespaces-1.png "Azure Kubernetes Service - Masterclass")

![Image](https://stacksimplify.com/course-images/azure-kubernetes-service-namespaces-2.png "Azure Kubernetes Service - Masterclass")

![Image](https://stacksimplify.com/course-images/azure-kubernetes-service-namespaces-3.png "Azure Kubernetes Service - Masterclass") 

## Pre-requisite Check (Optional)
- We should already have our AKS Cluster UP and Running.
- We should have configured our AKS Cluster credentials in command line to execute `kubectl` commands
```
# Configure AKS Cluster Credentials from command line
az aks get-credentials --resource-group multi-aks-rg --name aks-cluster
```
## Step-02: Namespaces Generic - Deploy in Dev1 and Dev2
### Create Namespace
```

```