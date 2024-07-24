# phpIpam

phpIpam is an open-source web IP address management application (IPAM).


## Installation

First, create the namespace:
```bash
kubectl create namespace phpipam
```
Then, create the secrets from the .env file:
```bash
kubectl --namespace phpipam create secret generic phpipam-secrets --from-env-file=.env
```
