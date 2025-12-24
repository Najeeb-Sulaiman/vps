## Deploying Airflow on K3S
### Requirements
- k3s
- Kubectl
- Helm

### Set up

Add the official repository of the Airflow Helm Chart:
```bash
helm repo add apache-airflow https://airflow.apache.org
```

Update the repo:
```bash
helm repo update
```

Create namespace airflow:
```bash
kubectl create namespace airflow
```
Check the namespace:
```bash
kubectl get namespaces
```

Install the Airflow Helm Chart:
```bash
helm install airflow apache-airflow/airflow --namespace airflow --debug
```

Get pods:
```bash
kubectl get pods -n airflow
```

Port-forward the Airflow UI to http://localhost:8080/ to confirm Airflow is working:
```bash
kubectl port-forward svc/airflow-api-server 8080:8080 --namespace airflow
```
