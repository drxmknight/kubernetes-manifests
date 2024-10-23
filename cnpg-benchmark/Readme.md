# Cloud Native PostgreSQL Benchmark

This repository contains the Kubernetes manifests needed to make a benchmark comparing the performance of 3 methods of deploying a PostgreSQL database on Kubernetes:
- Using PostgreSQL deployment using worker node volume
- Using PostgreSQL deployment using Longhorn as storage class
- (TO-DO) Using CloudNativePG Operator


## Prerequisites
- A running Kubernetes cluster with kubectl
- Longhorn installed on the cluster

## Install

1. Apply all the manifests:
```bash
kubectl apply -f .
```

This will create the following resources:
- A namespace called pg-benchmark
- A PostgreSQL database using a worker node volume (emptyDir)
- A PostgreSQL database using Longhorn as storage class
- A PostgreSQL database using the CloudNativePG Operator
- A Pod with pgbench to run the benchmark


2. To uninstall the resources:
```bash
kubectl delete -f .
```

## Benchmark

To run the benchmark, you can use the pgbench tool. The benchmark pod run the following commands:

1. First, database initialization:
```bash	
pgbench -i -s 100 -h localhost -U admin -d pg-benchmark
```

2. Run the benchmark with the following options:
```bash
pgbench -c 10 -j 1 -T 10 -h localhost -U admin -d pg-benchmark
```

pgbench options description:
- -s: value 1 means 100.000 rows in the table. 100 is a multiplier of the default value.
- -i: initialize the database
- -c: concurrent sessions (clients)
- -j: number of threads to distribute the clients
- -h: localhost
- -U: user name defined in the env POSTGRES_USER
- -d: database name
- -T: duration of the test in seconds
- -t: number of transactions per client. Cannot be used with -T


## Results

1. To check the benchmark results, you can check the logs of the pgbench pod:
```bash
kubectl logs benchmark benchmark-local
kubectl logs benchmark benchmark-longhorn 
```

Also, you can chech pgadmin to see