# Kubernetes - Secrets

## Step-01: Introduction
- Kubernetes Secrets let you store and manage sensitive information, such as passwords, OAuth tokens, and ssh keys.
- Storing confidential information in a Secret is safer and more flexible than putting it directly in a Pod definition or in a container image.

## Step-02: Create Secret for MySQL DB Password

```
# Mac
echo -n 'dummypassword' | base64

# URL: https://www.base64encode.org
```
### Create Kubernetes Secrets imperative approach
```
kubectl create secret generic mysql-secret --from-literal=MYSQL_PASSWORD=dummytodos --from-literal=MYSQL_ROOT_PASSWORD=dummypassword
kubectl create secret generic todo-web-app-secret --from-literal=RDS_PASSWORD=dummytodos
```
### Create Kubernetes Secrets manifest for MySQL Deployment
```yaml
apiVersion: v1
kind: Secret
metadata:
  labels:
    app: mysql
  name: mysql-secret
  namespace: default
type: Opaque
data:
  MYSQL_PASSWORD: ZHVtbXl0b2Rvcw==
  MYSQL_ROOT_PASSWORD: ZHVtbXlwYXNzd29yZA==
```

### Create Kubernetes Secrets manifest for Todo Web App Deployment
```yaml
apiVersion: v1
kind: Secret
metadata:
  labels:
    app: todo-web-app
  name: todo-web-app-secret
  namespace: default
type: Opaque
data:
  RDS_PASSWORD: ZHVtbXl0b2Rvcw==
```
## Step-03: Update secret in MySQL Deployment
```yaml

```