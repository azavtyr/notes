# Setup Gitea with Docker

This guide walks through the process of setting up **Gitea**, a lightweight self-hosted Git service, using **Docker** and optionally securing it with **Nginx Proxy Manager** for a custom domain and SSL.

## Prepare the Project Directory

First, create a directory:

```sh
mkdir gitea
cd gitea
```

## Docker Compose Setup

Create a docker-compose.yml file:

```yaml
---
services:
  server:
    image: docker.gitea.com/gitea:1.24.2
    container_name: gitea
    environment:
      - USER_UID=1000
      - USER_GID=1000
    restart: unless-stopped
    networks:
      - proxy
    volumes:
      - gitea-data:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro

volumes:
  gitea-data:

networks:
  proxy:
    external: true
```

## SSL via Nginx Proxy Manager (Optional)

If you'd like to access Gitea via a custom domain like `git.yourdomain.com` and secure it with HTTPS, follow these steps:

1. Access the Nginx Proxy Manager dashboard
1. Navigate to Proxy Hosts and click Add Proxy Host
1. Enter the following details:
    * Domain: `git.yourdomain.com`
    * Forward Hostname / IP: `gitea`
    * Forward Port: `3000`
1. Under the SSL tab:
    * Choose the existing certificate from the dropdown
    * Enable Force SSL and optionally HTTP/2.

Save the configuration.

## Complete Gitea Initial Setup via Browser

1. Open your browser and navigate to: `https://git.yourdomain.com`
1. The Gitea setup wizard will appear.
    * Most default settings are fine for a basic setup.
    * Optionally adjust configuration options (database, server settings, etc.).
1. Click Install Gitea.
1. After installation, you’ll be redirected to the login page.
1. Register a new user — this will be your admin account.

## Final Notes

After Gitea is up and running:

* Create a Repository: From the Gitea dashboard.
* Clone the Repo Locally: `git clone https://git.yourdomain.com/your-user/your-repo.git`
* Work with it Locally: Add, commit, and push changes just like with any Git remote.
