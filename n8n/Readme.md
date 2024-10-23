# n8n

Kubernetes manifests to deploy n8n, a workflow automation tool.

## Pre-requisites

1. Rename the .env.sample file to .env and fill in the required variables:
```bash
mv .env.sample .env
```


## Installation

1. First, create a namespace and the secret from the .env file:
```bash
kubectl create namespace n8n
kubectl create secret generic postgres-secret --from-env-file=.env --namespace=n8n
```

2. Deploy the resources:
```bash
kubectl apply -f .
```

3. To uninstall, delete the resources:
```bash
kubectl delete -f 01-namespace.yaml
```