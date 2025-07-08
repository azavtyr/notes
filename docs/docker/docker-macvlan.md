# Docker Macvlan

Macvlan network allows to assign a MAC address to a container, making it appear as a physical device on my home network.

* The container gets its own IP address from the physical network's subnet.
* Other devices on the local network can reach the container directly.

## Downsides

* Physical network interface must support promiscuous mode.
* Not well supported on Mac/Windows.

## Create a Macvlan network

```sh
docker network create -d macvlan \
    --subnet 192.168.0.0/24 \
    --gateway 192.168.0.1 \
    -o parent=eth0 custommacvlan
```

## Create a container connected to macvlan

```sh
docker run -itd --rm --network custommacvlan --ip 192.168.0.200 nginx
```

> [!IMPORTANT]
> You must manually assign an IP to each container, if you don't assign it, Docker will automatically assign one from the subnet, which may create a conflict with another device already using that IP on the network.
