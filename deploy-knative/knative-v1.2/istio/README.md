kubectl apply -l knative.dev/crd-install=true -f https://github.com/knative/net-istio/releases/download/knative-v1.2.0/istio.yaml
kubectl apply -f https://github.com/knative/net-istio/releases/download/knative-v1.2.0/istio.yaml

kubectl apply -f https://github.com/knative/net-istio/releases/download/knative-v1.2.0/net-istio.yaml
