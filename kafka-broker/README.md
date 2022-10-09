# Config Kafka Broker

### 配置Kafka Channel为默认的Channel类型

配置KafkaChannel为集群级别默认的Channel类型时，只需要将clusterDefault配置段中的配置指向KafkaChannel资源类型的引用即可。需要注意的是，spec字段中，numPartitions用于自定义Kafka Topic中的分区数量，而replicationFactor则用于指定Partition的复制因子，该数值不能大于Kafka Cluster中的节点数量。

```bash
kubectl apply -f default-ch-webhook.yaml
```

### 配置Kafka Broker为默认的Broker类型

需要配置KafkaBroker为集群全局默认的Broker类型时，只需要将ConfigMap/config-br-defaults中的clusterDefault配置段指定为对KafkaBroker资源类型的引用即可。需要注意的是，KafkaBroker在底层实现上要依赖于KafkaChannel，因此，创建KafkaBroker时，需要确认同一名称空间下KafkaChannel为默认的Channel类型，否则，创建操作可能会失败。

```bash
kubectl apply -f config-br-defaults.yaml
```

另外，KafkaBroker通常会依赖于ConfigMap/kafka-broker-config中的配置信息，请确保其numPartition和replicationFactor指定了正确的值。
