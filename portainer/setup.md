## Portainer Overview

### Portainer setup

Portainer can be deployed using this guide: [Install Portainer CE with Docker on Linux](https://docs.portainer.io/start/install-ce/server/docker/linux#introduction)

The deployment docker compose file can be found in this repo directory.

### Troubleshooting
- The url to access portainer must follow this format `https://<server ip>:9443`. The url must be http otherwise this error will be encountered: `Client sent an HTTP request to an HTTPS server.`
- This error: `Your Portainer instance timed out for security purposes. To re-enable your Portainer instance, you will need to restart Portainer` is encountered if a new Portainer installation wasn't completed within 5 minutes, which is a security measure to prevent unauthorized setup. To fix this, restart the Portainer container using the command `sudo docker restart portainer` in your terminal. For a new installation, the restart gives you another 5-minute window to complete the initial user setup in the UI.