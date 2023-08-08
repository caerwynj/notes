## Kubernetes

K8s - home cluster
- Prometheus, Grafana [arm64]
- Fluentd - logging
- Opensearch (elasticsearch, kibana)  [arm64]
- Argo CD  [amd64]
- Jupyterlab [amd64]
- Spark 
- Sqlite, PostgreSQL, litestream
- Redis [arm64]
- MinIO (S3 API) [arm64]
- ML Flow
- Airflow, dbt
- Container registry?
- NocoDB
- Istio
- nginx Ingress
- Inferno file services

Upgrade the whole cluster to arm64? amd64 x2, arm x2, arm64 x2.
Turn rpi6 and rpi7 into arm64 host.


AI
- What size models can be run on rpi in k8s.




# Helm


helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm search repo prometheus-community
helm install  ­-generate-name prometheus-community/kube-prometheus-stack  --set  nodeSelector."kubernetes\\.io/arch"=amd64

helm install  ­-generate-name  opensearch/opensearch  --set  nodeSelector."kubernetes\\.io/arch"=arm64


helm install --name elasticsearch elastic/elasticsearch --set  nodeSelector."kubernetes\\.io/arch"=arm64

helm upgrade --cleanup-on-fail \
  --install jupyter-stash jupyterhub/jupyterhub \
  --namespace jupyter \
  --create-namespace \
  --values config.yaml
  --set nodeSelector."kubernetes\\.io/arch"=amd64
  
Kubernetes configurations for inferno.  Helm charts.
Kubernetes manifests.  Store in artifacthub.io
Seperate repo for app configuration. GitOps.

## Kubectl
install juypter in kubernetes

kubectl create deployment image=jupyter/datascience-notebook jupyter
kubectl expose deployment jupyter --port=8888 
kubectl get service jupyter

numpy version of neural net following karpathy

install docker
build docker image of inferno-os

apt-key adv --keyserver keyserver.ubuntu.com --recv-keys <key>
install libseccomp2 on raspbian to get around issues with apt-get update inside container.


Update the .dockerignore for arm container.

Inferno network.
- signer; open ports x4; get keys from etcd;  create users; login when running keyfs
- styx file server; open styx port; getauthinfo
- httpd server
- export ports
- kubernetes services for httpd and styx
- registry service
- venti service with NFS persistence volume
- owen service, worker nodes
object store
