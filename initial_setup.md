### SSH Setup
Follow the guide in the Hetzner's documentation to setup: [Setting up an ssh key](https://community.hetzner.com/tutorials/howto-ssh-key#step-1---generating-an-ssh-key)

The refer to the [connecting to your server](https://docs.hetzner.com/cloud/servers/getting-started/connecting-to-the-server/) guide.

### Secure Root Access
After connecting, create a new user in other to avoid using the root user for login:
```bash
sudo apt update && sudo apt upgrade -y
sudo adduser najeeb
sudo usermod -aG sudo devops
```

Log in with the new user:
```bash
ssh najeeb@<the-server-ip>
```

### Install Core Tooling
### Docker & Docker Compose
Follow this guide in the official docker documentation to install docker and other docket utilities:

[Install Docker using the `apt` repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)

### DevOps Utilities
Install some server utilities:
```bash
sudo apt install -y git make unzip htop net-tools
```