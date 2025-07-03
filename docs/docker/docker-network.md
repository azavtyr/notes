# Docker Network

Docker networking allows containers to communicate with each other and external services. It provides the ability to isolate, secure, and scale applications. Docker's networking system is pluggable, which means you can choose the right tool for the job. There are several built-in drivers:

* `bridge`
* `host`
* `macvlan`
* `ipvlan`
* `none`
* `overlay`

> [!NOTE]
> Optional: A container may be connected to different types of networks. For example, an IPvlan network to provide internet access and a bridge network for access to local services.

## List all networks

`docker network ls`

## Connect a container to a network

`docker network connect my_network my_container`

## Disconnect a container from a network

`docker network disconnect my_network my_container`

## Exposing Ports

`docker run -itd --name my_container -p 80:80 my_image`

## Inspect a specific network

`docker network inspect my_network`