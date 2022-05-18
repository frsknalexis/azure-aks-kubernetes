# Kubernetes - Requests and Limits

## Step-01: Introduction
- We can specify how much each container in a pod needs the resources like CPU & Memory.
- When we provide this information in our pod, the scheduler uses this information to decide which node to place the Pod on based on availability of k8s worker Node CPU and Memory Resources.
- When you specify a resource limit for a Container, the kubelet enforces those `limits` so that the running container is not allowed to use more of that resource than the limit you set.
- The kubelet also reserves at least the `request` amount of that system resource specifically for that container to use.

![Image](https://stacksimplify.com/course-images/azure-kubernetes-service-resources-requests-limits-1.png "Azure Kubernetes Service - Masterclass")

![Image](https://stacksimplify.com/course-images/azure-kubernetes-service-resources-requests-limits-2.png "Azure Kubernetes Service - Masterclass") 
