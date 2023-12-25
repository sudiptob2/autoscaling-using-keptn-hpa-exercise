# Keptn Metrics Provider exercise
This exercise will demonstrate how to use the Keptn Metrics Provider to scale an application based on a custom metric.

## Install Kind Cluster
For this demo, I will use the `kind` local Kubernetes cluster. You can install it by following the instructions [here](https://kind.sigs.k8s.io/docs/user/quick-start/).

## Install Prometheus
For this demo, I will use Prometheus as the metrics collector. I have provided a command for installing Prometheus in the Makefile. Navigate to the `install` directory and run the following command:
```bash
make install-prometheus
```

## Deploy Podtato Head
Let's deploy the PodtatoHead `podtato-head-entry` service. We will use this app for our demo. The manifest file for PodtatoHead is provided in the `demo` directory. Navigate to the `demo` directory and run the following command:
```bash
kubectl apply -f podtato-head-entry.yaml
```
After applying, please ensure that the application is up and running:
```bash
kubectl get pods -n podtato-kubectl
```
You can also port-forward the PodtatoHead service to your local machine:
```bash
kubectl port-forward svc/podtato-head-entry 9000:9000 -n podtato-kubectl
```
Now, visit `http://localhost:9000/` in your browser, and you should see the PodtatoHead app.

![img.png](assets/img.png){:width="200px"}

## Verify Prometheus Is Collecting Metrics
Let's verify that Prometheus is collecting metrics from our PodtatoHead app. First, port-forward Prometheus to your local machine:
```bash
kubectl port-forward svc/prometheus-kube-prometheus-prometheus 9090:9090 -n monitoring
```

You can also verify that Prometheus is collecting metrics for our PodtatoHead app by entering `sum(kube_pod_container_resource_limits{resource="cpu"})` in the `Expression` field and clicking on `Execute`.

## Install Keptn Metrics Operator
Navigate to the `install` directory and run the following command:
```bash
make install-keptn
```

## Apply Keptn Metrics
Now apply `keptn-metrics.yaml` and `keptn-metrics-provider.yaml` from the `demo` directory:
```bash
kubectl apply -f keptn-metrics.yaml
kubectl apply -f keptn-metrics-provider.yaml
```
View metrics by running the following command:
```bash
kubectl get KeptnMetric -n podtato-kubectl 
```

## Apply Horizontal Pod Autoscaler
Now that we are able to retrieve the value of our metric and have it stored in our cluster in the status of our KeptnMetric custom resource, we can configure a `HorizontalPodAutoscaler` to make use of this information and scale our application automatically:
```bash
kubectl apply -f hpa.yaml
```
Sometimes it takes a while for the pods to scale up. If you don't see the pods scaling up, please wait for a while.

## Notes
- keptn metric docs https://keptn.sh/latest/docs/reference/crd-reference/metricsprovider/#files
- Keptn + HPA example https://keptn.sh/latest/docs/use-cases/hpa/#set-up-the-horizontalpodautoscaler

