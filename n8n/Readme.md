# n8n

Kubernetes manifests to deploy n8n, a workflow automation tool.

## Pre-requisites

1. Rename the .env.sample file to .env and fill in the required variables:
```bash
mv .env.sample .env
```

# Installation 

1. Set up the environment variables in the `.env` file and load them:
```bash
export $(grep -v '^#' .env | xargs)
```

2. Validate the configuration:
```bash
kubectl kustomize . | envsubst
```

3. Deploy the resources:
```bash
kubectl kustomize . | envsubst | kubectl apply -f -
```
