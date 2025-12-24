## K3s Setup Guide
### Install K3s (Traefik enabled)
This installs a full Kubernetes distro, with Traefik as the default ingress controller:
```bash
curl -sfL https://get.k3s.io | sh -
```
Check that it’s running:
```bash
sudo kubectl get nodes
sudo kubectl get pods -A
```
`Traefik` should be running in the `kube-system` namespace.

### Save kubeconfig for local access to avoid using `sudo`
Using `sudo` with `kubectl/helm` commands is often unnecessary if you move your K3s kubeconfig to your home directory. This allows you to run Helm commands as a regular user, which is more secure:
```bash
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $(id -u):$(id -g) ~/.kube/config
```
Set the environment variable (add this to your ~/.bashrc or ~/.zshrc):
```bash
export KUBECONFIG=~/.kube/config
```
Now you can run: 
```bash
kubectl get nodes
helm install airflow apache-airflow/airflow ...
```
without `sudo`.
