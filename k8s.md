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

# Inferno on K8s

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

How to setup a signer in k8s?
Need password for keyring.
auth/createsignerkey caerwyn
svc/auth (requires login for /keydb/keys
auth/changelogin (requires info for user)
We don't need to create a lot of users.  We just need one or two.
So we basically need to share an already prepared signerkey, and keys file.

How to setup sytx?
Store keyring in Secret.
That is all it needs. Then run svc/net

kubectl create configmap scripts --from-file=startup

k8s persistence store using NFS.

why can't I attach or exec to an inferno pod in k8s?

use Ansible to install docker on all rpi nodes.
create docker image with docker client so I can run builds on the cluster.
create Job or Task to run in k8s.
create CronJob

webgrab -r http://10.43.191.89:8080/magic/echo
mount -A tcp!10.43.128.85!registry /mnt/registry
mount -A tcp!10.43.120.118 /n/remote
 