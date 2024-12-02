# AIrflow-helm

# Create cluster using kind
kind create cluster --name airflow-cluster --config kind-cluster.yaml
kubectl cluster-info

# Airflow installation using helm chart
helm repo add apache-airflow https://airflow.apache.org
helm repo update
helm search repo airflow

# Create namespace for installation
kubectl create -f namespace.yaml

# Installation command using yaml file
helm install airflow apache-airflow/airflow -n mvdls3perf -f deployment-helm.yaml

# Check newly created pods in mvdls3perf namespace
kubectl get pods -n mvdls3perf -o wide
kubectl get nodes -n mvdls3perf -o wide

# Forward the traffic to external port
# Alternatively create Service - Nodeport type
kubectl port-forward svc/airflow-webserver 8080:8080 -n mvdls3perf

# Export values.yaml file; It can be used for redeployment -
# - Update airflow version
# - Update executor type
# - GitSync

helm show values apache-airflow/airflow > values.yaml
helm upgrade --install airflow apache-airflow/airflow -n mvdls3perf -f values.yaml --debug
helm ls -n mvdls3perf 

# Create secret file for using gitSync
kubectl create secret generic airflow-ssh-git-secret --from-file=gitSshKey=C:\Users\dheer\.ssh\id_rsa -n mvdls3perf
kubectl get secrets -n mvdls3perf
helm upgrade --install airflow apache-airflow/airflow -n mvdls3perf -f values.yaml --debug

# get scheduler logs for troubleshooting errors
kubectl logs airflow-scheduler-5f6bf74dc-zzj29 -c git-sync -n mvdls3perf

kubectl port-forward svc/airflow-webserver 8080:8080 -n mvdls3perf

