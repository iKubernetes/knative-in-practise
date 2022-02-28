# Sequence

### Steps
\# Knative Services for steps
~$ kn service apply sq-appender-01 --image ikubernetes/appender --env MESSAGE=" - Handled by SQ-01" -n event-demo
~$ kn service apply sq-appender-02 --image ikubernetes/appender --env MESSAGE=" - Handled by SQ-02" -n event-demo
~$ kn service apply sq-appender-03 --image ikubernetes/appender --env MESSAGE=" - Handled by SQ-03" -n event-demo

### Sink
~$ kn service apply event-display --image=ikubernetes/event_display --port 8080 --scale-min 1 -n event-demo



curl -v "http://sq-demo-kn-sequence-0-kn-channel.event-demo.svc.cluster.local"  -X POST -H  "Content-Type: application/cloudevents+json" \
            -d '{"id": "0", "specversion": "1.0", "type": "com.magedu.sayhi", "source": "Curl", "data": {"message":"Hello Knative Eventing Flow"}}'


curl -v "http://sq-demo-kn-sequence-0-kn-channel.event-demo.svc.cluster.local" \
-X POST \
-H "Ce-Id: 0" \
-H "Ce-Specversion: 1.0" \
-H "Ce-Type: com.magedu.sayhi" \
-H "Ce-Source: Curl" \
-H "Content-Type: application/json" \
-d '{"message":"Hello Knative Eventing Flow"}'



POD_EVENT=$(kubectl get pods -l serving.knative.dev/service=event-display -o jsonpath={.items[*].metadata.name} -n event-demo)
