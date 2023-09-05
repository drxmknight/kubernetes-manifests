# Jenkins 

Manifest files to deploy a jenkins server in kubernetes


## Prerequisites

If you are **not using Longhorn CSI**, then deploy the volumes as local volumes. The nodeAffinity must be configured:

To to it, configure the env var with the name of the node.
```
JENKINS_NODE="NODE_NAME"
```

## Deploy

Apply the manifests files:
```bash
kubectl apply -f .
```

Get the service info:
```bash
kubectl -n jenkins get svc jenkins
```

To access the webapp, you can:
- Access from a node in: **CLUSTER-IP:PORT**
- Access direct to the node: **WORKER_IP:NodePort**

To setup jenkins, see the logs from the pod:
```bash
kubectl -n jenkins logs JENKINS_POD_NAME | grep -i password -A5
```

Copy the password and insert it in the web UI.


## Links

- Jenkins official guide: https://www.jenkins.io/doc/book/installing/kubernetes/
- Jenkins manifest files: https://devopscube.com/setup-jenkins-on-kubernetes-cluster/