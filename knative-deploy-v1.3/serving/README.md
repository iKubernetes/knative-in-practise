# Knative Serving

### Install the Knative Serving component

```bash
kubectl apply -f serving-crds.yaml
kubectl apply -f serving-core.yaml
```

### Install a networking layer (Istio)

Install a properly configured Istio by running the command:

```bash
kubectl apply -l knative.dev/crd-install=true -f istio.yaml
kubectl apply -f istio.yaml
```

Install the Knative Istio controller by running the command:

```bash
kubectl apply -f net-istio.yaml
```

Fetch the External IP address or CNAME by running the command:

```bash
kubectl --namespace istio-system get service istio-ingressgateway
```

### Verify the installation

Monitor the Knative components until all of the components show a STATUS of Running or Completed. You can do this by running the following command and inspecting the output:

```bash
kubectl get pods -n knative-serving
```

### Install optional Serving extensions

Knative also supports the use of the Kubernetes Horizontal Pod Autoscaler (HPA) for driving autoscaling decisions.

Install the components needed to support HPA-class autoscaling by running the command:

```bash
kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.3.0/serving-hpa.yaml
```
