# Port 53 Conflict When Setting Up Pi-hole on Ubuntu

When setting up **Pi-hole on Ubuntu**, you may encounter **a port conflict on port 53**, which prevents Pi-hole from starting. This happens because `systemd-resolved` is enabled by default and listens on `127.0.0.53:53`, occupying the DNS port needed by Pi-hole.

## Resolve the Conflict and Configure DNS

Disable the stub resolver:

```sh
sudo sed -r -i.orig 's/#?DNSStubListener=yes/DNSStubListener=no/g' /etc/systemd/resolved.conf
```

Change the `/etc/resolv.conf` symlink to point to `/run/systemd/resolve/resolv.conf`:

```sh
sudo sh -c 'rm /etc/resolv.conf && ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf'
```

After making these changes, restart systemd-resolved:

```sh
systemctl restart systemd-resolved
```

To set the docker host's nameservers edit the netplan `/etc/netplan/*.yaml`:

```yaml
network:
  version: 2
  ethernets:
    ens160:  # Replace with your actual interface name
      dhcp4: true
      dhcp4-overrides:
        use-dns: false
      nameservers:
        addresses: [127.0.0.1]
```

Apply changes:

```sh
sudo netplan apply
```
