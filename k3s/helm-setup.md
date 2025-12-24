## Install Helm Using APT

Setting up Helm on an Ubuntu server can be done using several methods. Using the package manager(APT) is the recommended approach because it ensures Helm is updated alongside your other system software.

See the Helm documentation for the different installation options:
https://helm.sh/docs/intro/install/

1. Install prerequisites:
```bash
sudo apt-get install curl gpg apt-transport-https --yes
```
2. Add the GPG key:
```bash
curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
```
3. Add the repository:
```bash
echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
```
4. Install Helm:
```bash
sudo apt-get update
sudo apt-get install helm
```
