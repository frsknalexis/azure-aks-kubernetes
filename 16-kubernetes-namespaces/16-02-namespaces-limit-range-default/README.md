# Kubernetes Namespaces - LimitRange - Declarative using YAML

![Image](https://stacksimplify.com/course-images/azure-kubernetes-service-namespaces-limit-range.png "Azure Kubernetes Service - Masterclass")

## Pre-requisite Check (Optional)
- We should already have our AKS Cluster UP and Running.
- We should have configured our AKS Cluster credentials in command line to execute `kubectl` commands
```
# Configure AKS Cluster Credentials from command line
az aks get-credentials --resource-group multi-aks-rg --name aks-cluster
```
## Step-01: Create Namespace manifest
- **Important Note:** File name starts with `00-`  so that when creating k8s objects namespace will get created first so it don't throw an error.