# Langfuse

## Install

To install Langfuse, follow the steps below:

1. Add the Langfuse Helm repository:
```bash
helm repo add langfuse https://langfuse.github.io/langfuse-k8s
helm repo update
```

2. Create the namespace for Langfuse:
```bash
kubectl create namespace langfuse
```

2. Create a Kubernetes secret for the Langfuse credentials:
```bash
LANGFUSE_SECRET=$(openssl rand -base64 32 | tr -dc 'a-zA-Z')
kubectl --namespace langfuse create secret generic langfuse \
--from-literal=salt=$LANGFUSE_SECRET \
--from-literal=encryption-key=$LANGFUSE_SECRET \
--from-literal=nextauth-secret=$LANGFUSE_SECRET \
--from-literal=postgres-password=$LANGFUSE_SECRET \
--from-literal=redis-password=$LANGFUSE_SECRET \
--from-literal=clickhouse-password=$LANGFUSE_SECRET
```

3. Install Langfuse using Helm:
```bash
helm upgrade langfuse langfuse/langfuse --install \
--set langfuse.ingress.enabled=true \
--set langfuse.ingress.hosts[0].host=langfuse.str.local \
--set langfuse.ingress.hosts[0].paths[0].path=/ \
--set langfuse.ingress.hosts[0].paths[0].pathType=Prefix \
--set langfuse.ingress.className=nginx \
--set langfuse.salt.secretKeyRef.name=langfuse \
--set langfuse.salt.secretKeyRef.key=salt \
--set langfuse.nextauth.secret.secretKeyRef.name=langfuse \
--set langfuse.nextauth.secret.secretKeyRef.key=nextauth-secret \
--set postgresql.auth.existingSecret=langfuse \
--set postgresql.auth.secretKeys.userPasswordKey=postgres-password \
--set clickhouse.auth.existingSecret=langfuse \
--set clickhouse.auth.existingSecretKey=clickhouse-password \
--set redis.auth.existingSecret=langfuse \
--set redis.auth.existingSecretPasswordKey=redis-password \
--set clickhouse.replicaCount=1
```


## Uninstall
1. To uninstall Langfuse, delete the Helm release:
```bash
helm uninstall langfuse --namespace langfuse
```

2. Delete the namespace:
```bash
kubectl delete namespace langfuse
```