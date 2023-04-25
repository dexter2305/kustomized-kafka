# Kafka installation using kustomize

Kafka requires zookeeper for this version. `kustomize` is used to install kafka.

# About installation
Installation creates the following

- Zookeeper and kafka instances are deployed in namespace `kafka`.
- Kafka producer and consumer pods are deployed in namespace  `kafka`. These are pods are useful for testing.

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
- Install broker, zookeper, kafka-producer, kafka-consumer.
- Kafka-producer and Kafka-consumer pods will be used only for testing. 
```bash
k apply -k . 
```

> Installing individual components deploys components in the default namespace.

# Uninstall
```bash
k delete -k .
```