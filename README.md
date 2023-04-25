# Kafka installation using kustomize

Kafka requires zookeeper for this version. `kustomize` is used to install kafka.

# About installation
Installation creates the following

- Zookeeper and kafka instances are deployed in namespace `kafka`.
- Kafka producer and consumer pods are deployed in namespace  `kafka`. These are pods are useful for testing.

# Syntax Check

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

# Install
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

## Useful commands using kafka-console shell scripts
Bash into broker pod or producer pod or consumer pod to try out the following commands. Below commands uses env variables set in Pod manifest
### Topic management
- Create 
  ```bash
  kafka-topic $bsso --create --topic mytopic --partitions 10
  ```
- List 
  ```bash
  kafka-topic $bsso --list
  ```
- Describe 
  ```bash
  kafka-topic $bsso --describe --topic mytopic
  ```
### Producer
- Env var `$bsso` refers to the bootstrap server option and its value. Refer Pod manifest for its values.
- Create producer 
  ```bash
  kafka-console-producer $bsso --topic mytopic < 10.msgs.input
  ```
- Create producer with key parser
  Produces messages with key, value separated by `=`
  ```bash
  kafka-console-producer $bsso --property parse.key=true --property key.separator== --topic mytopic < 10.msgs.input
  ```

### Consumer
- Var `$bsso` refers to the bootstrap server option and its value. Refer Pod manifest for its values.
- Var `$consumer_props` is optional. Enables printing of the partition, key and value. Used here for better understanding.  Refer pod manifest for its value.
- Create consumer with a group
  ```bash
  kafka-console-consumer $bsso $consumer_props --group mygroup.name --topic mytopic
  ```
- List consumer groups
  ```bash
  kafka-consumer-group $bsso --list
  ```
- Describer consumer groups
  ```bash
  kafka-consumer-group $bsso --list --group mygroup.name
  ```
  Using `--members` switch lists all the members of the consumer group.
  ```bash
  kafka-consumer-group $bsso --list --group mygroup.name --members
  ```

## References
- [conduktor.io](https://www.conduktor.io/kafka/what-is-apache-kafka/)
- [Baeldung - listing kafka consumer groups](https://www.baeldung.com/ops/listing-kafka-consumers)