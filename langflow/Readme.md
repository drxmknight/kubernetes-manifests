# LangFlow Helm Chart

## Deployment Instructions

To install Langfuse, follow the steps below:

1. Add the Langfuse Helm repository:
```bash
helm repo add langflow https://langflow-ai.github.io/langflow-helm-charts
helm repo update

```

2. Install Langflow using Helm:
```bash
helm upgrade langflow-ide langflow/langflow-ide --install --namespace langflow --create-namespace \
--set ingress.enabled=true \
--set ingress.hosts[0].host=langflow.ehlab.local \
--set ingress.hosts[0].paths[0].path=/ \
--set ingress.hosts[0].paths[0].pathType=Prefix \
--set ingress.tls[0].hosts[0]=langflow.ehlab.local \
--set ingress.tls[0].secretName=ehlab.local-cert \
--set langflow.backend.env[0].name=LANGFLOW_DATABASE_URL \
--set langflow.backend.env[0].value="sqlite:////app/db/langflow.db"
```

```bash
helm upgrade langflow-ide langflow/langflow-ide --install --namespace langflow --create-namespace  --values values.yaml
```

## Uninstall

1. To uninstall, delete the Helm release:
```bash
helm uninstall langflow-ide --namespace langflow
```

2. Delete the namespace:
```bash
kubectl delete namespace langflow
```