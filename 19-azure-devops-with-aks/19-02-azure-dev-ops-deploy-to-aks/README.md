# Azure DevOps - Build, Push to ACR and Deploy to AKS

## Step-01: Pre-requisites
- We should have Azure AKS Cluster Up and Running.
```
# Configure Command Line Credentials
az aks get-credentials --resource-group multi-aks-rg-2 --name aks-cluster-2

# Verify Nodes
kubectl get nodes
kubectl get nodes -o wide
```

## Step-02: Introduction
- Add a Deployment Pipeline in Azure Pipelines to Deploy newly built docker image from ACR to Azure AKS

![Image](https://www.stacksimplify.com/course-images/azure-devops-pipelines-deploy-to-aks.png "Azure AKS Kubernetes - Masterclass")

## Step-03: Create Pipeline for Deploy to AKS
- Go to Pipelines -> Create new Pipeline
- Where is your code ?: Github
- Select a Repository: "select your repo" (frsknalexis/azure-devops-github-deploy-to-aks)
- Configure your pipeline: Deploy to Azure Kubernetes Service
- Select Subscription: Suscripción de Azure 1 (select your subscription)
- Provide username and password (Azure cloud admin user)
- Deploy to Azure Kubernetes Service
    - Cluster: aks-cluster-2
    - Namespace: existing (default)
    - Container Registry: acrfordevopsaks
    - Image Name: nginxaksapp1
    - Service Port: 80
- Click on **Validate and Configure**
- Review your pipeline YAML
    - Change Pipeline Name: 01-docker-build-and-push-to-acr-deploy-to-aks-pipeline.yml
- Click on **Save and Run**
- Commit Message: Pipeline-1: Docker, Build, Push and Deploy to AKS with Azure Pipelines
- Commit directly to master branch: check
- Click on  **Save and Run**

## Step-04: Verify Build and Deploy logs
- Build stage should pass. Verify logs
- Deploy stage should pass. Verify logs

## Step-05: Verify Build and Deploy pipeline logs
- Go to Pipeline -> Verify logs
```
# Verify Pods
kubectl get pods

# Get Public IP
kubectl get svc

# Access Application
http://<public-ip-from-get-service-output>
```

## Step-06: Rename Pipeline Name
- Go to pipeline -> Rename / Move
- Name: 02-docker-build-and-push-to-acr-deploy-to-aks
- Folder: App-Pipelines
- Refresh till changes reflect
- Verify -> Pipelines -> Click on **All** tab

## Step-07: Make Changes to index.html and Verify
```
# Pull
git pull

# Make changes to index.html
Change version to V3

# Commit and Push
git commit -am "[update index.html to V3]"
git push

# Verify Build and Deploy logs
- Build stage logs
- Deploy stage logs
- Verify ACR Repository

# List Pods (Verify Age of Pod)
kubectl get pods

# Get Public IP
kubectl get svc

# Access Application
http://<public-ip-from-get-service-output>
```

## Step-08: Disable Pipeline
- Go to Pipeline -> 02-docker-build-and-push-to-acr-deploy-to-aks -> Settings -> Disable

## Step-09: Review Pipeline code
- Click on Pipeline -> Edit Pipeline
- Review pipeline code
- Review Service Connections
```yaml
# Deploy to Azure Kubernetes Service
# Build and push image to Azure Container Registry; Deploy to Azure Kubernetes Service
# https://docs.microsoft.com/azure/devops/pipelines/languages/docker

trigger:
- master

resources:
- repo: self

variables:

  # Container registry service connection established during pipeline creation
  dockerRegistryServiceConnection: 'c9f7c7a4-c79d-41f9-8141-ae51b1e96fd5'
  imageRepository: 'nginxaksapp1'
  containerRegistry: 'acrfordevopsaks.azurecr.io'
  dockerfilePath: '**/Dockerfile'
  tag: '$(Build.BuildId)'
  imagePullSecret: 'acrfordevopsaks20834207-auth'

  # Agent VM image name
  vmImageName: 'ubuntu-latest'


stages:
- stage: Build
  displayName: Build stage
  jobs:
  - job: Build
    displayName: Build
    pool:
      vmImage: $(vmImageName)
    steps:
    - task: Docker@2
      displayName: Build and push an image to container registry
      inputs:
        command: buildAndPush
        repository: $(imageRepository)
        dockerfile: $(dockerfilePath)
        containerRegistry: $(dockerRegistryServiceConnection)
        tags: |
          $(tag)

    - upload: manifests
      artifact: manifests

- stage: Deploy
  displayName: Deploy stage
  dependsOn: Build

  jobs:
  - deployment: Deploy
    displayName: Deploy
    pool:
      vmImage: $(vmImageName)
    environment: 'frsknalexisazuredevopsgithubdeploytoaks-1374.default'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: KubernetesManifest@0
            displayName: Create imagePullSecret
            inputs:
              action: createSecret
              secretName: $(imagePullSecret)
              dockerRegistryEndpoint: $(dockerRegistryServiceConnection)

          - task: KubernetesManifest@0
            displayName: Deploy to Kubernetes cluster
            inputs:
              action: deploy
              manifests: |
                $(Pipeline.Workspace)/manifests/deployment.yml
                $(Pipeline.Workspace)/manifests/service.yml
              imagePullSecrets: |
                $(imagePullSecret)
              containers: |
                $(containerRegistry)/$(imageRepository):$(tag)
```

## Step-10: Clean-Up Apps in AKS Cluster
```
# Delete Deployment
kubectl get deployments
kubectl delete deployment nginxaksapp1

# Delete Service
kubectl get svc
kubectl delete svc nginxaksapp1
```