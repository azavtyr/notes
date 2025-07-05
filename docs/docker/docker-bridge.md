# Docker Bridge

The bridge driver creates a private network for containers on the host machine. When you start Docker, it automatically creates a default bridge network on the host machine, represented by a virtual interface (`docker0`), which started containers connect to unless otherwise specified. Containers on the same bridge network can talk to each other via IP addresses. However, if you want to access a service (like a webserver) running in a container from your host machine, you'll need to expose ports to the host.

## User Defined Custom Bridge

Uses the same bridge driver, but you define it instead. There are some advantages to using this type of network:

* It provides automatic DNS resolution between containers, which means that containers can resolve each other by name or alias.
- User Defined Bridges provide better isolation. Only containers connected to the same user-defined bridge can communicate with each other.

### Create a user-defined bridge

```sh
docker network create custombridge
```

### Create a container connected to the user-defined bridge

```sh
docker run -itd --rm --network custombridge alpine
```
