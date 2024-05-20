# Logging with google cloud logging plugin with grafana for GKE #

##################### Install grafana Using Helm ##################################################################

Kubectl create ns Logging

helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install grafana grafana/grafana --set persistence.enabled=true,persistence.size=10Gi


#Get admin password
kubectl get secret --namespace Logging grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo


#Access Grafana dashboard using port forward
kubectl port-forward service/grafana 3000:80


#
