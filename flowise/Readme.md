# Flowise Kubernetes Deployment

Kubernetes manifests for deploying Flowise, a low-code platform for building AI applications.

## Pre-requisites

1. Cppy the .env.sample file to .env and fill in the required variables:
```bash
cp .env.sample .env
```

# Deployment Steps

1. Set up the environment variables in the `.env` file and load them:
```bash
export $(grep -v '^#' .env | xargs)
```

2. Validate the configuration:
```bash
kubectl kustomize . | envsubst > compiled.yaml
```

3. Deploy the resources:
```bash
kubectl apply -f compiled.yaml
```
