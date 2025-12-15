# ArgoCD GitOps Example

This repository contains examples of how to deploy applications to a Kubernetes cluster using ArgoCD.

## Structure

The `apps` directory contains the ArgoCD Application definitions organized by application:

- `apps/homepage/`: Landing page with links to all services
  - `application.yaml`: ArgoCD Application manifest
  - `manifests/`: Kubernetes manifests (ConfigMap with HTML, Deployment, Service, Ingress)
- `apps/httpbin/`: Plain YAML example deploying httpbin (HTTP request & response service)
  - `application.yaml`: ArgoCD Application manifest
  - `manifests/`: Kubernetes manifests (Deployment, Service, Ingress)
- `apps/podinfo-helm/`: Helm chart example deploying podinfo
  - `application.yaml`: ArgoCD Application manifest pointing to the Helm repository

The `apps/kustomization.yaml` file lists all the applications that ArgoCD should deploy.

## Usage

1. **Install ArgoCD** in your Kubernetes cluster.
2. **Create a new ArgoCD application** that points to this repository:

   ```yaml
   apiVersion: argoproj.io/v1alpha1
   kind: Application
   metadata:
     name: my-apps
     namespace: argocd
   spec:
     project: default
     source:
       repoURL: 'https://github.com/pllanderas/k8s-gitops-demo.git'
       path: apps
       targetRevision: HEAD
     destination:
       server: 'https://kubernetes.default.svc'
       namespace: default
     syncPolicy:
       automated:
         prune: true
         selfHeal: true
   ```

3. **Access the applications:**
   - **Homepage**: https://home.193.146.210.202.nip.io (Landing page with links to all services)
   - `httpbin`: http://httpbin.193.146.210.202.nip.io
   - `podinfo-helm`: https://podinfo-helm.193.146.210.202.nip.io
   - `ArgoCD`: https://argocd.193.146.210.202.nip.io
   - `Harbor`: https://harbor.193.146.210.202.nip.io

   The applications use nip.io for DNS resolution, which automatically resolves to the IP address in the hostname.
