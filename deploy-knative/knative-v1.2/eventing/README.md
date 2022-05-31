kubectl apply -f https://github.com/knative/eventing/releases/download/knative-v1.2.0/eventing-crds.yaml

kubectl apply -f https://github.com/knative/eventing/releases/download/knative-v1.2.0/eventing-core.yaml


kubectl get pods -n knative-eventing


kubectl apply -f https://github.com/knative/eventing/releases/download/knative-v1.2.0/in-memory-channel.yaml


kubectl apply -f https://github.com/knative/eventing/releases/download/knative-v1.2.0/mt-channel-broker.yaml

