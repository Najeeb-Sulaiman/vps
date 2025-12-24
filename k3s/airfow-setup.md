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

## Debug
### Timeout or `airflow-run-airflow-migrations` error
If the database migration job (`airflow-run-airflow-migrations`) failed to complete within the default 5-minute timeout window. If it is a fresh install and the migration is stuck, it’s often because the database was not initialized correctly or is unreachable due to a networking issue.

- Check the status of your database pod:
```bash
kubectl get pods -n airflow
```

- If the status is `CrashLoopBackOff`: Check the database logs immediately: kubectl logs -l app=postgresql -n airflow. 
- If the database pod is `Pending` or `ImagePullBackOff`: The database hasn't even started. Check for image pull errors or insufficient resource issues.

For `ImagePullBackOff` error on the PostgreSQL pod, it is a known issue in late 2025, caused by the Apache Airflow Helm chart's dependency on an older Bitnami PostgreSQL subchart.

Specifically, the default version of the chart (e.g., 1.18.0) tries to pull a Bitnami image tag (like 16.1.0-debian-11-r15) that has been moved to a "legacy" registry or removed from Docker Hub entirely.

**How to Fix the ImagePull Error:**

To resolve this, you must explicitly set a valid PostgreSQL image tag during your Helm installation. The easiest workaround is to use the latest tag or a verified stable tag.

Delete the failed release and try the install again with a longer timeout and Postgres image `latest` tag:
```bash
helm uninstall airflow -n airflow

helm install airflow apache-airflow/airflow \
  --namespace airflow \
  --set postgresql.image.tag="latest" \
  --debug \
  --timeout 10m0s
```
Alternatively, specific tag like 16.4.0 can be used, if you want to avoid using `latest` in production.

- If it's a simple timeout error with no `ImagePullBackOff` error on the PostgreSQL pod or the `airflow-migrations` is in ContainerCreating state. Just delete the failed release and try the install again with a longer timeout(`--timeout 10m0s`).


## Values.yaml
Configuration file of the Helm chart and so, of the Airflow deployment

### Get the Chart values
```bash
helm show values apache-airflow/airflow > values.yaml
To update the Airflow deployment
```
### Check the current revision
```bash
helm ls -n airflow
```

#### Upgrade the chart
```bash
helm upgrade --install airflow apache-airflow/airflow -n airflow -f values.yaml --debug
```

### Check after
```bash
helm ls -n airflow
```

## How to access the Airflow UI from your laptop
By default, `kubectl port-forward` only listens on `localhost (127.0.0.1)` of the machine where you run the command. If you ran it on a VPS, it is listening for connections inside the VPS, not from your laptop.

**To access the Airflow UI from your laptop:**

A secure tunnel from your laptop to the VPS can be created. This makes your laptop think Airflow is running locally.

1. Keep the command running on your VPS:
```bash
kubectl port-forward svc/airflow-api-server 8080:8080 --namespace airflow
```
2. Open a NEW terminal on your local laptop and run:
```bash
ssh -L 8080:localhost:8080 your-user@<vps-ip>
```

3. Access on your laptop: Open your browser and go to http://localhost:8080.

For more details on how these tunnels work, you can refer to the SSH.com Local Port Forwarding Guide:
https://www.ssh.com/academy/ssh/tunneling-example