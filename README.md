# Kafka installation using kustomize

Kafka requires zookeeper for this version. `kustomize` is used to install kafka.

# About installation

- Zookeeper and kafka are deployed in namespace `kafka`.
- Creates a single instance of zookeeper
- Creates a single instance of broker
- Creates a producer and consumer pod in namespace `lab`. This is for debugging purpose.


# Installation steps  

## Apply syntax check 
```bash
k kustomize .
``` 

## Dry run on the client size
```bash 
k apply -k . --dry-run=client
```

## Dry run on the server side
Cases where the namespace is missing in the cluster are identified by this test
```bash
k apply -k . --dry-run=server
```

## Install
- Install broker and zookeper
```bash
k apply -k . 
```

- Install producer and consumer for testing
```bash
k apply -k producer
k apply -k consumer
```



