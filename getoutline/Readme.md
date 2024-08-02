# GetOutline

## Pre-requisites

1. Generate a random hex key and paste it in the SECRET_KEY variable in the secret.yaml file:
```bash
openssl rand -hex 32
```

2. Create a .env file with the variables required: 
- [https://github.com/outline/outline/blob/main/.env.sample](https://github.com/outline/outline/blob/main/.env.sample)


## Installation

Deploy the resources:
```bash
kubectl apply -f 01-namespace.yaml
kubectl create secret generic env-secret --from-env-file=.env --namespace=getoutline
kubectl apply -f .
```

## Uninstall

To uninstall, delete the resources:
```bash
kubectl delete -f 01-namespace.yaml
```