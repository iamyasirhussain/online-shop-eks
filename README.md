# online-shop-eks

# Online Shop - EKS Deployment

This project deploys an online shop application to AWS EKS with HTTPS enabled.

## Prerequisites

- AWS CLI configured
- kubectl installed
- Helm installed
- Domain name pointed to EKS load balancer
- Docker image available on DockerHub

## Quick Start

1. Clone this repo:
```bash
git clone <your-repo-url>
cd online-shop-eks/k8s


# Create namespace
kubectl apply -f namespace.yml

# Deploy application
kubectl apply -f deployment.yml
kubectl apply -f service.yml

# Install cert-manager (for HTTPS)
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.14.4 \
  --set installCRDs=true

# Create cluster issuer (replace email)
kubectl apply -f cluster-issuer.yml

# Deploy ingress with HTTPS
kubectl apply -f ingress.yml

Configure DNS:

Get ELB address: kubectl get svc -n ingress-nginx ingress-nginx-controller

Create A records in your DNS provider for both:

@ → ELB_IP

www → ELB_IP

Access Your Application
HTTP: http://yourdomain.com

HTTPS: https://yourdomain.com (auto-redirects from HTTP)
