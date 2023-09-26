# Docker Registry

Deploy a private docker registry to upload images


## Prerequisites

Create certificates to secure communication. The name is in the format SERVICE_NAME.NAMESPACE:
```bash
mkdir -p certs; 
openssl req -x509 -days 3650 -newkey rsa:4096 -nodes -sha256 \
-keyout certs/docker-registry.key \
-out certs/docker-registry.cert \
-addext "subjectAltName = DNS:docker-registry.docker-registry"
```

Create a secret in the cluster to store certs:
```bash
kubectl create ns docker-registry
kubectl create secret tls registry-cert --namespace=docker-registry --cert=certs/docker-registry.cert --key=certs/docker-registry.key
```

Apply the manifests:
```bash
kubectl apply -f .
```


Copy the certs to the node where you build the images. Must be in the docker folder to secure the connection to the registry:
```bash
export BUILD_NODE=NODENAME
rsync -r certs/ root@$BUILD_NODE:/etc/docker/certs.d/docker-registry.docker-registry/
```

As a self signed certificate, also must be added to the trust anchors. In this case, for centos:
```bash
rsync -r certs/ root@$BUILD_NODE:/etc/pki/ca-trust/source/anchors/
```

Update ca trust and restart docker service:
```bash
ssh root@$BUILD_NODE 'update-ca-trust && systemctl restart docker'
```

To use, you must tag the images using the ip of the registry service:
```bash
docker tag IMAGE:TAG docker-registry.docker-registry/IMAGE:TAG
docker push docker-registry.docker-registry/IMAGE:TAG
```

To validate the images:
```bash
curl -k https://docker-registry.docker-registry/v2/_catalog
```

## Uninstall

To delete all the resources, delete the namespace:
```bash
kubectl delete namespace docker-registry
```