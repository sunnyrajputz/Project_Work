# ArgoCD CI/CD Process in github and rollout in Github.

ArgoCD is a popular GitOps tool for continuous delivery on Kubernetes. It automates the deployment of the desired application states in Git repositories.
Here's a step-by-step guide to set up a CI/CD pipeline using ArgoCD with GitHub for both application deployment and rollout management.

This guide sets up a CI/CD pipeline using ArgoCD and GitHub, automating the deployment and rollout of applications on a Kubernetes cluster. Adjust the paths and repository details as needed for your specific use case.

# Prerequisites

**Kubernetes Cluster:** A running Kubernetes cluster.
**ArgoCD Installation:** ArgoCD should be installed on your Kubernetes cluster.
**GitHub Repository:** A repository containing your application manifests (YAML files).
**kubectl:** The Kubernetes command-line tool configured to interact with your cluster.
**ArgoCD CLI:** The ArgoCD command-line interface installed locally.

## Step 1: Install ArgoCD on Kubernetes
First, install ArgoCD on your Kubernetes cluster:
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
## Step 2: Access ArgoCD UI
Expose the ArgoCD API server using a LoadBalancer service or port forwarding:
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
**Note:** Access the ArgoCD UI at https://localhost:8080.
## Step 3: Log in to ArgoCD
Get the initial admin password:
```
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```
**Note:** Log in using the username **"admin"** and the password retrieved above.
## Step 4: Create an ArgoCD Project
Create an ArgoCD project in your GitHub repository. This project will manage the deployment of your application. The project can be defined as follows:
**project.yaml**
```
apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata:
  name: my-project
  namespace: argocd
spec:
  sourceRepos:
    - https://github.com/my-user/my-repo
  destinations:
    - namespace: default
      server: https://kubernetes.default.svc
  description: My application project
```
# Apply the project configuration:
```
kubectl apply -f project.yaml
```
## Step 5: Define Your Application in ArgoCD
Create an ArgoCD application resource to define how your application should be deployed:
**application.yaml**
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-application
  namespace: argocd
spec:
  project: my-project
  source:
    repoURL: 'https://github.com/my-user/my-repo'
    targetRevision: HEAD
    path: 'path/to/manifests'
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: 'default'
  syncPolicy:
    automated:
      prune: true
      selfHeal: true

```
# Apply the application configuration:
```
kubectl apply -f application.yaml
```
## Step 6: Configure GitHub Actions for CI
Create a GitHub Actions workflow to automate the CI process. Create a .github/workflows/ci.yml file in your repository:
```
name: CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Kubernetes tools
      uses: azure/setup-kubectl@v1
      with:
        version: 'latest'

    - name: Deploy to Kubernetes
      run: |
        kubectl apply -f path/to/manifests

```


## Rollout Plans
## Step 7: Argo Rollout for Blue-Green or Canary Deployments
To perform blue-green or canary deployments, use Argo Rollouts. First, install Argo Rollouts in your cluster:
```
kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://raw.githubusercontent.com/argoproj/argo-rollouts/stable/manifests/install.yaml
```

# Define a Rollout resource in your application manifests: rollout.yaml

```
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: my-rollout
  namespace: default
spec:
  replicas: 2
  strategy:
    canary:
      steps:
      - setWeight: 20
      - pause: {}
      - setWeight: 50
      - pause: {}
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-image:latest
        ports:
        - containerPort: 80

```
# Apply the Rollout resource:

```
kubectl apply -f rollout.yaml

```
## Step 8: Monitor and Manage Rollouts
Use the Argo Rollouts kubectl plugin to monitor and manage the rollouts:

```
kubectl argo rollouts get rollout my-rollout
kubectl argo rollouts promote my-rollout

```
