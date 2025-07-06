# Docker IPvlan

IPvlan network allows the host machine to share the MAC address with containers. So all containers MAC address will match the host's one, but they all will have different IP addresses and appear as physical devices on home network.

## Create a IPvlan

```sh
docker network create -d ipvlan \
    --subnet=192.168.0.0/24 \
    --gateway=192.168.0.1 \
    -o parent=eth0 customipvlan
```

## Create a container connected to IPvlan

```sh
docker run -itd --rm --network customipvlan --ip 192.168.0.200 nginx
```

> [!IMPORTANT]
> As with Macvlan, you must manually assign an IP to each container, if you don't assign it, Docker will automatically assign one from the subnet, which may create a conflict with another device already using that IP on the network.

