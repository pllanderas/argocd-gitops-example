# ArgoCD GitOps Example

This repository contains examples of how to deploy applications to a Kubernetes cluster using ArgoCD.

## Structure

The `apps` directory contains the ArgoCD Application definitions. Each file in this directory defines an application that ArgoCD will manage.

- `apps/podinfo.yaml`: An example of a plain YAML deployment. The Kubernetes manifests for this application are located in `apps/base/podinfo`.
- `apps/podinfo-helm.yaml`: An example of a Helm-based deployment. This application is deployed directly from a Helm repository.

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
       repoURL: 'https://github.com/edgeContinuum/meo-services-gitops.git'
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
   - `podinfo`: http://podinfo.example.com
   - `podinfo-helm`: http://podinfo-helm.example.com

   You will need to configure your DNS or `/etc/hosts` file to point these hostnames to your Ingress controller.
