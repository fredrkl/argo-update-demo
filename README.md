# Argo CD update

This repo tests the Argo CD update process when ArgoCD is managed by the _App
of Apps_ pattern.

## Bootstrap

[_Kind_](https://kind.sigs.k8s.io/) is a tool to easily create a local K8s
cluster using only Docker. This is useful for testing and development. The
following steps will create a local K8s cluster and install the necessary tools
for testing the bootstrap procedure using _Kind_.

```bash
kind create cluster --name bootstrap
```

By default Kind will make the kubectl current context point to the new cluster.

## Install ArgoCD

```bash
helm repo add argo https://argoproj.github.io/argo-helm
kubectl create namespace argocd
```

Then install ArgoCD using Helm:

```bash
helm install argocd argo/argo-cd -n argocd
```

## Bootstrap App-of-Apps

Then finally we kick of the _GitOps App-of-Apps_ process by running the
following command:

```bash kubectl kustomize kustomize/overlays/{environment} | k apply -f -```

You might see some errors when you run the command. This is because the CRDs
are not yet installed. You can ignore these errors. The CRDs will be installed
by the ArgoCD application itself, and the instances of them picked up by the
ArgoCD application.

## Exterpiment

The purpose of this repository is to test the ArgoCD update process. The ArgoCD
HELM chart is updated to a new version using the _App-of-Apps pattern_.
