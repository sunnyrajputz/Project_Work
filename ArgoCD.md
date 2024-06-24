# ArgoCD CI/CD Process in github and rollout in Github.

ArgoCD is a popular GitOps tool for continuous delivery on Kubernetes. It automates the deployment of the desired application states in Git repositories.
Here's a step-by-step guide to set up a CI/CD pipeline using ArgoCD with GitHub for both application deployment and rollout management.

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
