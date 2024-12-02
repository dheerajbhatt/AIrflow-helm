kind create cluster --name airflow-cluster --config kind-cluster.yaml
kubectl cluster-info

helm repo add apache-airflow https://airflow.apache.org
helm repo update
helm search repo airflow

kubectl create -f namespace.yaml
helm install airflow apache-airflow/airflow -n mvdls3perf -f deployment-helm.yaml

kubectl get pods -n mvdls3perf -o wide
kubectl get nodes -n mvdls3perf -o wide

kubectl port-forward svc/airflow-webserver 8080:8080 -n mvdls3perf

helm show values apache-airflow/airflow > values.yaml
helm upgrade --install airflow apache-airflow/airflow -n mvdls3perf -f values.yaml --debug
helm ls -n mvdls3perf 

kubectl create secret generic airflow-ssh-git-secret --from-file=gitSshKey=C:\Users\dheer\.ssh\id_rsa -n mvdls3perf
kubectl get secrets -n mvdls3perf
helm upgrade --install airflow apache-airflow/airflow -n mvdls3perf -f values.yaml --debug
kubectl logs airflow-scheduler-5f6bf74dc-zzj29 -c git-sync -n mvdls3perf

kubectl port-forward svc/airflow-webserver 8080:8080 -n mvdls3perf

