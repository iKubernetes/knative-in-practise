# Install Knative Eventing

### 
1. Install the required custom resource definitions (CRDs) and core components of Eventing by running the command:
```bash
kubectl apply -f eventing-crds.yaml
kubectl apply -f eventing-core.yaml
```

2. Verify the installation
```bash
kubectl get pods -n knative-eventing
```

3. Install an in-memory implementation of Channel by running the command:
```bash
kubectl apply -f in-memory-channel.yaml
```

4. Install this implementation of Broker by running the command:
```bash
kubectl apply -f mt-channel-broker.yaml
```

To customize which Broker Channel implementation is used, update the following ConfigMap to specify which configurations are used for which namespaces:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: config-br-defaults
  namespace: knative-eventing
data:
  default-br-config: |
    # This is the cluster-wide default broker channel.
    clusterDefault:
      brokerClass: MTChannelBasedBroker
      apiVersion: v1
      kind: ConfigMap
      name: imc-channel
      namespace: knative-eventing
    # This allows you to specify different defaults per-namespace,
    # in this case the "some-namespace" namespace will use the Kafka
    # channel ConfigMap by default (only for example, you will need
    # to install kafka also to make use of this).
    namespaceDefaults:
      some-namespace:
        brokerClass: MTChannelBasedBroker
        apiVersion: v1
        kind: ConfigMap
        name: kafka-channel
        namespace: knative-eventing
```

The referenced imc-channel and kafka-channel example ConfigMaps would look like:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: imc-channel
  namespace: knative-eventing
data:
  channel-template-spec: |
    apiVersion: messaging.knative.dev/v1
    kind: InMemoryChannel
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: kafka-channel
  namespace: knative-eventing
data:
  channel-template-spec: |
    apiVersion: messaging.knative.dev/v1alpha1
    kind: KafkaChannel
    spec:
      numPartitions: 3
      replicationFactor: 1
```
