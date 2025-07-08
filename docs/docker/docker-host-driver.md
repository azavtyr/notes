# Docker Host Driver

The host driver removes isolation between the container and the Docker host, and directly connects to the host's network.

* The container doesn't get its own IP address, it uses the host's IP.
* There is no need to expose ports, any service running in the container is directly accessible via the host's ports.

> [!CAUTION]
> * This driver removes network isolation.
> * Not supported or not fully supported on Mac/Windows. (Only Linux)

## Create a container connected to host network

```sh
docker run -itd --rm --network host --name webserver nginx
```

Open a web-browser and type the host's IP address in the URL bar.
