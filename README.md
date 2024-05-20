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



![image](https://github.com/Shubham2194/grafana-GKE/assets/83746560/24e1d0f8-c4b1-4a71-a7a7-039f112779fd)


Now head over to Connections> Add new connection (install it) and and choose google-cloud-logging-datasource as new connection 
Add Json file in jwt section (Create a service account on GCP and give GKE full access and Create json key) and choose this key.
save and test  (turn on as default )


![image](https://github.com/Shubham2194/grafana-GKE/assets/83746560/43db0ae1-9e7e-41b1-8daf-8fb22d967f71)


We can see now in Data sources section Google cloud Logging has been added as default source.


![image](https://github.com/Shubham2194/grafana-GKE/assets/83746560/f79a8942-cdf2-42da-a8a4-8c6795bf5974)


Our grafana is communicating with Google cloud Logging resource 
######
Lets Explore our added data source 
Look after Explore section and Choose google-cloud-logging-datasource as source(if not fluctuate)

![image](https://github.com/Shubham2194/grafana-GKE/assets/83746560/70774554-9a0f-4911-98b3-1be2b49ae622)

QUERY >>

resource.type="k8s_container"
resource.labels.project_id="XYZ"
resource.labels.location="asia-southeast1"
resource.labels.cluster_name="dev-gke"
resource.labels.namespace_name="nginx"
resource.labels.container_name= "nginx"
severity>=DEFAULT

You can algo Add and create dashboard with the query ,to view lOGS diretly there 
In Explore section Choose Add to dashboard and You are done !!! 
