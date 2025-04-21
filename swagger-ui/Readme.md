# swagger-editor

Kubernetes manifests for deploying Swagger UI.

## Pre-requisites

1. Rename the .env.sample file to .env and fill in the required variables:
```bash
cp .env.sample .env
```

# Installation 

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
