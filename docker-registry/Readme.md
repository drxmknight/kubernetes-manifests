# Docker Registry

Deploy a private docker registry in Kubernetes to upload images


## Prerequisites

- Have a certificate and private key to secure communication. In this case, we are using cert-manager to generate the certificate.


- Copy the certs to the node where you build the images. Must be in the docker folder to secure the connection to the registry:
```bash
export BUILD_NODE=k8s-wrk-02
ssh root@$BUILD_NODE 'mkdir -p /etc/docker/certs.d/registry.str.local'
kubectl get secret str-local-cert -o jsonpath='{.data.tls\.crt}' | base64 -d | ssh root@$BUILD_NODE 'cat > /etc/docker/certs.d/registry.str.local/tls.crt'
```

## Install

Apply the manifests:
```bash
kubectl apply -f .
```

## Upload Images
To use, you must tag the images using the ip of the registry service:
```bash
docker tag IMAGE:TAG registry.str.local/IMAGE:TAG
docker push registry.str.local/IMAGE:TAG
```

To validate the images:
```bash
curl -k https://registry.str.local/v2/_catalog
```

## Uninstall

To delete all the resources, delete the namespace:
```bash
kubectl delete namespace docker-registry
```