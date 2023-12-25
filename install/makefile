.PHONY: install-keptn
install-keptn:
	helm repo add keptn https://charts.lifecycle.keptn.sh
	helm repo update
	helm upgrade --install keptn keptn/keptn \
	   -n keptn-system --create-namespace --wait

.PHONY: install-prometheus
install-prometheus:
	helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
	helm repo update
	helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace
