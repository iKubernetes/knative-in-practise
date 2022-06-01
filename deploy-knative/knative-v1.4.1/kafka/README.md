# Kafka Channel and Kafka Broker

## Strimzi Kafka

1. Install Strimzi Operator
```bash
kubectl create namespace kafka
kubectl create -f 'https://strimzi.io/install/latest?namespace=kafka' -n kafka
```

2. Deploy Kafka Cluster
```bash
kubectl apply -f https://strimzi.io/examples/latest/kafka/kafka-ephemeral-single.yaml -n kafka
```

Other deployment examples:
- kafka-ephemeral.yaml
- kafka-persistent-single.yaml
- kafka-persistent.yaml
- kafka-jbod.yaml


  https://github.com/strimzi/strimzi-kafka-operator/tree/main/examples/kafka

3. Verify the installation
```bash
kubectl get pods -n kafka -l app.kubernetes.io/instance=my-cluster
```

Example output:
```
NAME                                          READY   STATUS    RESTARTS        AGE
my-cluster-entity-operator-559b5d6d89-x87zd   3/3     Running   0               9m
my-cluster-kafka-0                            1/1     Running   0               9m23s
my-cluster-zookeeper-0                        1/1     Running   0               11m
my-cluster-zookeeper-1                        1/1     Running   0               11m
my-cluster-zookeeper-2                        1/1     Running   0               11m
```

4. Send and receive messages

Once the cluster is running, you can run a simple producer to send messages to a Kafka topic (the topic will be automatically created):

```
kubectl -n kafka run kafka-producer -ti --image=quay.io/strimzi/kafka:0.29.0-kafka-3.2.0 --rm=true --restart=Never -- bin/kafka-console-producer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic
```

And to receive them in a different terminal you can run:

```
kubectl -n kafka run kafka-consumer -ti --image=quay.io/strimzi/kafka:0.29.0-kafka-3.2.0 --rm=true --restart=Never -- bin/kafka-console-consumer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic --from-beginning
```

## Install the Apache Kafka Channel for Knative

1. Install the Kafka Source

```bash
kubectl apply -f https://storage.googleapis.com/knative-nightly/eventing-kafka/latest/source.yaml
```

2. Install the Kafka "Consolidated" Channel

```bash
kubectl apply -f https://storage.googleapis.com/knative-nightly/eventing-kafka/latest/channel-consolidated.yaml
```

## Apache Kafka Broker

1. Install the Kafka controller by running the following command:

```bash
kubectl apply -f https://github.com/knative-sandbox/eventing-kafka-broker/releases/download/knative-v1.4.2/eventing-kafka-controller.yaml

```

2. Install the Kafka Broker data plane by running the following command:

```bash
kubectl apply -f https://github.com/knative-sandbox/eventing-kafka-broker/releases/download/knative-v1.4.2/eventing-kafka-broker.yaml
```
