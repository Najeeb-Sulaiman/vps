## Portainer Overview

### Portainer setup

Portainer can be deployed using this guide: [Install Portainer CE with Docker on Linux](https://docs.portainer.io/start/install-ce/server/docker/linux#introduction)

The deployment docker compose file can be found in this repo directory.

### Troubleshooting
- The url to access portainer must follow this format `https://<server ip>:9443`. The url must be http otherwise this error will be encountered: `Client sent an HTTP request to an HTTPS server.`