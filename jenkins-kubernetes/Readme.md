# Jenkins 

Manifest files to deploy a jenkins server in kubernetes

## Deploy

Create a .env file in the folder with the name of the worker node where you want to deploy the server:
```
export JENKINS_NODE="NODE_NAME"
```

Apply the manifests files:
```bash
kubectl apply -f jenkins-kubernetes
```

Get the service info:
```bash
kubectl -n jenkins get svc jenkins-service
```

To access the server, you can:
- Access from a node in: **CLUSTER-IP:PORT**
- Access direct to the node: **WORKER_IP:NodePort**

To setup jenkins, see the logs from the pod:
```bash
kubectl -n jenkins logs JENKINS_POD | grep -i password -A5
```

Copy the password and insert it in the webapp.