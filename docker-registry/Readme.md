# Docker Registry

Deploy a private docker registry to upload images


## Prerequisites

Create certificates to secure communication. For ease of use, we will use a static clusterIP and add it as SAN:
```bash
mkdir -p certs; 
openssl req -x509 -days 3650 -newkey rsa:4096 -nodes -sha256 \
-keyout certs/docker-registry.key \
-out certs/docker-registry.cert \
-addext "subjectAltName = IP:10.100.0.200"
```

Create a secret in the cluster to store certs:
```bash
kubectl create ns docker-registry
kubectl create secret tls registry-cert --namespace=docker-registry --cert=certs/docker-registry.cert --key=certs/docker-registry.key
```

If you want to use push images from another app (ie. jenkins), you have to create the secret in that namespace:
```bash
kubectl create secret tls registry-cert --namespace=jenkins --cert=certs/docker-registry.cert --key=certs/docker-registry.key
```

Apply the manifest:
```bash
kubectl apply -f 01-docker-registry.yaml
```

Copy the certs to the node where you build the images. Must be in the docker folder to secure the connection to the registry:
```bash
rsync -r certs/ root@BUILD_NODE:/etc/docker/certs.d/10.100.0.200/
```

As a self signed certificate, also must be added to the trust anchors. In this case, for centos:
```bash
rsync -r certs/ root@BUILD_NODE:/etc/pki/ca-trust/source/anchors/
```

Update ca trust and restart docker service:
```bash
ssh root@BUILD_NODE 'update-ca-trust && systemctl restart docker'
```

To use, you must tag the images using the ip of the registry service:
```bash
docker tag IMAGE:TAG 10.100.0.200/IMAGE:TAG
docker push 10.100.0.200/IMAGE:TAG
```

## Uninstall

To delete all the resources, delete the namespace:
```bash
kubectl delete namespace docker-registry
```