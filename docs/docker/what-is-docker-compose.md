# Docker Compose

**Docker Compose** is a tool that allows you to run multiple containers with a single configuration file.

The `docker-compose.yml` is the config file that defines all the services and parameters for containers to apply and run.

## Explaining docker-compose.yml

```yml
---
version: '3' # Compose file format version

services: # Defines all services / containers
  service-name: # Custom name
    image: # Docker image to build a container
    volumes: # Mount host directories as data volumes
    ports: # Expose ports to host
    environment: # Add environment variables

networks: # Networks for services to connect
volumes: # Define shared data volumes
```
