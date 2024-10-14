# Certmanager Guide

Certmanager is a platform for managing certificates within a Kubernetes environment. 


## Certmanager Installation

1. To install Certmanager, apply the provided YAML manifest to create the necessary resources in the "cert-manager" namespace.

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.3/cert-manager.yaml
```

For detailed installation instructions, refer to the [Cert-manager Documentation](https://cert-manager.io/docs/installation/kubectl/).


2. Generate a Certificate Authority (CA) and its private key.
```bash
openssl req -x509 -nodes -days 3650 -newkey rsa:4096 -keyout ca.key -out ca.crt
```

3. Create a Kubernetes Secret to store the CA certificate.
```bash
kubectl create secret tls ca-cert --namespace=cert-manager --cert=ca.crt --key=ca.key
```

4. Create a **ClusterIssuer** object to enable the issuance and signing of certificates for different applications in Kubernetes:


5. Create a **Certificate** resources with wildcard domain, referencing the previously defined ClusterIssuer.


6. Add TLS Configuration to Ingress: 

Example Ingress resource for Jenkins to include TLS configuration with the created certificate.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  namespace: app
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/proxy-body-size: "100M"
spec:
  ingressClassName: nginx
  rules:
  - host: app.lab.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app
            port:
              number: 8080
  tls:
  - hosts:
    - app.lab.local
    secretName: lab-local-cert
```

Now, your application should be accessible via HTTPS with the configured SSL certificate.