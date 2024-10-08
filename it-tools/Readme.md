# IT Tools App - Kubernetes Deployment

This README provides instructions on how to deploy the IT Tools app to a Kubernetes cluster.

## Prerequisites

- A Kubernetes cluster (local or cloud-based)
- `kubectl` installed and configured

## Deployment Steps

1. **Clone the Repository**: Start by cloning the IT Tools repository to your local machine.
  
2. **Apply Kubernetes Manifests**: Use `kubectl` to apply the manifest files, which will deploy the application to the Kubernetes cluster.  
```bash
kubectl apply -f .
``` 