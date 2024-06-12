# Zabbix

## Install

First, create the namespace:
```bash
kubectl create namespace zabbix
```

Then, create the secrets from the .env file:
```bash
kubectl --namespace zabbix create secret generic zabbix-secrets --from-env-file=.env
```

Then, apply the manifests:
```bash
kubectl apply -f .
```
