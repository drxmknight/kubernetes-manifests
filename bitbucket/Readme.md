# Bitbucket

The proyect details the steps to deploy a Bitbucket instance in a Kubernetes cluster.

## Pre-requisites

1. Add the helm chart repository:
```bash
helm repo add atlassian-data-center https://atlassian.github.io/data-center-helm-charts
helm repo update
```

2. Obtain the values.yaml. Recommended to use the local bitbucket-values.yaml file:
```bash
helm show values atlassian-data-center/bitbucket > helm/values.yaml
```


## Install

1. Deploy the needed resources:
```bash
kubectl create namespace bitbucket
kubectl -n bitbucket create secret generic bitbucket --from-env-file=.env
kubectl apply -f .
```

2. Install the helm chart:
```bash
helm upgrade bitbucket atlassian-data-center/bitbucket \
--install --namespace bitbucket \
--values helm/bitbucket-values.yaml
```

## Expose to the internet

1. To expose the service to internet, use Cloudflare or any other service. In this case, we will use Cloudflare tunnel. Create the application and expose it to a domain.

2. Create the ingress resource pointing to the cloudflare domain:
```bash
kubectl apply -f cloudflare/03-ingress-cloudflare.yaml
```


## Docs

- https://hub.docker.com/r/atlassian/bitbucket
- https://atlassian.github.io/data-center-helm-charts/userguide/INSTALLATION/


