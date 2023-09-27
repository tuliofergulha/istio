# Service Mesh With Istio

This project is a simple example of how you can create, test and use for your studies the Istio in a kuberntes enviroment.

## Technologies

* k3d - https://k3d.io/
* Istio - https://istio.io/

## K3D Configuration
To get start, it's necessary to configure your environment. First, you'll need to install the tool k3d. This is a wrapper of kubernetes that we can run localy. These tool was choosen because it's very easy to install and they have a easy way to configure map ports between our computer and the kubenertes cluster, but, if you prefer, the examples can be used and configured with minikube, kind, or other kubernetes tool.
After install k3d, just need to run the following commands:

### Create cluster 
* `k3d cluster create -p "8000:30000" --agents 2`

### Set context
* `kubectl config use-context k3d-k3s-default`

### Verify nodes from your cluster
* `kubectl get nodes`

## Istio CTL Installation

### Download
Follow this documentation: https://istio.io/latest/docs/setup/getting-started/#download
After download, configure istioctl as env var from your computer.

### Installing Istio on cluster

* `istioctl install`

Use the following command `kubectl get ns` to retrieve the namespaces of your cluster.
Verify if there is a namespace named `istio-system`.

Check the pods that istio created in your cluster:
* `kubectl get pods -n istio-system`

Check the service `default` and the `istio-system`:
* `kubectl get svc`
* `kubectl get svc -n istio-system`

### Apply istio sidecar into namespace
* `kubectl label namespace default istio-injection=enabled` 

## Setup Integrations
Documentation link: https://istio.io/latest/docs/ops/integrations/

### Prometheus
* `kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.19/samples/addons/prometheus.yaml`

### Kiali
* `kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.19/samples/addons/kiali.yaml`
* `istioctl dashboard kiali`

### Jaeger
* `kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.19/samples/addons/jaeger.yaml`

### Grafana
* `kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.19/samples/addons/grafana.yaml`

## Setup Fortio - Load Tests
* `kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.19/samples/httpbin/sample-client/fortio-deploy.yaml`
* `export FORTIO_POD=$(kubectl get pods -l app=fortio -o 'jsonpath={.items[0].metadata.name}')`
* `kubectl exec "$FORTIO_POD" -c fortio -- fortio load -c 2 -qps 0 -t 200s -loglevel Warning http://istio-app-service:8000`

### Bash - Load test
* `while true; do curl -s http://localhost:8000 | grep -o '<h1>[^<]*</h1>' | sed 's/<[^>]*>//g'; sleep 0.5; done`

## Commands
* `kubectl apply -f deployment.yaml`
* `kubectl apply -f deployment.yaml`
* `kubectl get pods`
* `kubectl delete deploy istio-app`
* `kubectl describe pods ${POD_NAME}`

