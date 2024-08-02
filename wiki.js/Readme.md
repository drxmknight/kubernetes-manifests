# Wiki.js

Wiki.js install guide in kubernetes.

## Prerequisites

- Deploy a single instance for initial setup. After, we can scalate it.
- PostgreSQL is a requirement for multiple replicas.


## Installation

Add helm chart:
  
```bash
helm repo add requarks https://charts.js.wiki
helm repo update requarks
```

Install the chart:
  
```bash
helm upgrade wikijs requarks/wiki --install --namespace wikijs --create-namespace \
--set ingress.enabled=true \
--set ingress.hosts[0].host=wiki.str.local \
--set ingress.hosts[0].paths[0].path=/ \
--set ingress.hosts[0].paths[0].pathType=Prefix \
--set ingress.tls[0].secretName=wikijs-cert \
--set ingress.tls[0].hosts[0]=wiki.str.local \
--set ingress.className=nginx
```

## Uninstall

To uninstall the chart:

```bash
helm delete wikijs
kubectl delete pvc data-wikijs-postgresql-0
```
