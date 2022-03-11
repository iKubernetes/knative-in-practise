# KafkaSource Demo

### Create KafkaTopic topic01 in default namespace
```
kubectl create -f 01-kafka-topic-demo.yaml
```

### Create Knative Service event-display
```
kn service apply event-display --image ikubernetes/event_display --port 8080 --scale-min 1
```

### Create KafkaSource demo
```
kubectl apply -f 02-kafkasource-demo.yaml
```

### Test
Send some message to Kafka/my-cluster
```
kubectl run kafka-producer -it --image=quay.io/strimzi/kafka:0.28.0-kafka-3.1.0 --rm=true --restart=Never -- bin/kafka-console-producer.sh --broker-list my-cluster-kafka-bootstrap.kafka:9092 --topic topic01
```
