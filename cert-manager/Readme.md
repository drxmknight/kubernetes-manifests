# Certmanager Guide

Certmanager for managing certificates within a Kubernetes environment. 


## Installation

1. Apply the provided YAML manifest to create the necessary resources in the "cert-manager" namespace. For detailed installation instructions, refer to the [Cert-manager Documentation](https://cert-manager.io/docs/installation/kubectl/):
```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.3/cert-manager.yaml
```

2. Configure the domain environment variable:
```bash
export DOMAIN=lab.local
```

2. Generate a Certificate Authority (CA) and its private key:
```bash
openssl req -x509 -nodes -days 3650 -newkey rsa:4096 -keyout ${DOMAIN}.key -out ${DOMAIN}.ca
```

3. Create a Kubernetes Secret to store the CA certificate.
```bash
kubectl create secret tls ${DOMAIN}-ca-cert --namespace=cert-manager --cert=${DOMAIN}.ca --key=${DOMAIN}.key
```

4. Create a **ClusterIssuer** object to enable the issuance and signing of certificates for different applications in Kubernetes:
```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: ${DOMAIN}-ca-issuer
  namespace: cert-manager
spec:
  ca:
    secretName: ${DOMAIN}-ca-cert
EOF
```

5. Create a **Certificate** resources with wildcard domain, referencing the previously defined ClusterIssuer.
```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: ${DOMAIN}-cert
  namespace: cert-manager
spec:
  secretName: ${DOMAIN}-cert
  dnsNames:
    - "*.${DOMAIN}"
  subject:
    organizations:
      - "${DOMAIN}"
  issuerRef:
    name: ${DOMAIN}-ca-issuer
    kind: ClusterIssuer
  commonName: "*.${DOMAIN}"
  secretTemplate:
    annotations:
      reflector.v1.k8s.emberstack.com/reflection-allowed: "true"
      reflector.v1.k8s.emberstack.com/reflection-auto-enabled: "true"
EOF
```


## Usage 

Add TLS Configuration to Ingress. Example Ingress resource for Jenkins to include TLS configuration with the created certificate.

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