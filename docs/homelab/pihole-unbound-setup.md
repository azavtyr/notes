# Pi-hole Setup with Docker Compose and Unbound

This guide documents how I set up **Pi-hole** with **Unbound** using Docker Compose. The goal is to self-host DNS resolution and eliminate reliance on third-party DNS providers like Google or Cloudflare — improving privacy and blocking ads network-wide.

## Create Project Directory

Start by creating a directory to store config files:

```sh
mkdir pihole && cd pihole
```

This folder will hold the `docker-compose.yml` and any volume data.

## Docker Compose Setup

I'm using the `mpgirro/pihole-unbound` Docker image, which combines Pi-hole with Unbound — a recursive DNS resolver — in a single container.

Unbound queries root DNS servers directly, ensuring your DNS requests aren't logged or tracked by external providers.

```yaml
---
services:
  service:
    container_name: pihole
    image: mpgirro/pihole-unbound
    hostname: ${HOSTNAME}
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80/tcp"
      - "443:443/tcp"
      - "67:67/udp"
    environment:
      - TZ=${TZ}
      - FTLCONF_webserver_api_password=${FTLCONF_webserver_api_password}
      - FTLCONF_dns_upstreams=127.0.0.1#5335
      - FTLCONF_dns_dnssec="true"
      - FTLCONF_dns_listeningMode=single
    networks:
      proxy:
      pihole_network:
        ipv4_address: '192.168.0.150'
    volumes:
      - config_pihole:/etc/pihole
    restart: unless-stopped

volumes:
    - config_pihole:/etc/pihole
    restart: unless-stopped

volumes:
  config_pihole:

networks:
  proxy:
    external: true
  pihole_network:
    driver: macvlan
    driver_opts:
      parent: ens18
    ipam:
      config:
        - subnet: 192.168.0.0/24
          gateway: 192.168.0.1

```

> [!NOTE]
> Make sure the external network `proxy` already exists, or create it using `docker network create proxy`.

### Environment Variables

Set environment variables in the `.env` file:

```sh
export HOSTNAME="$(hostname)" # Use system hostname
export TZ="Region/Your_City"
export FTLCONF_webserver_api_password="your-secure-password"
```

Then start the container:

```sh
docker compose up -d
```

## SSL via Nginx Proxy Manager (Optional)

To serve the Pi-hole web interface securely via HTTPS:

1. Set up Nginx Proxy Manager.
1. Add a new proxy host:
    * Domain: `dns.yourdomain.com`
    * Forward Hostname / IP: the IP of the Pi-hole container's host
    * Forward Port: `80`
1. Enable SSL and request a Let's Encrypt certificate.

> Now you can access Pi-hole via `https://dns.yourdomain.com`.

## Configure OpenWRT to Use Pi-hole DNS

To have your router forward all DNS queries to Pi-hole:

1. Go to OpenWRT Web Interface
1. Navigate to: `Network > DHCP and DNS`
1. Under `DNS forwards`, enter your VM IP (e.g. `192.168.1.100`)
1. Click `Save & Apply`

This ensures all devices using your router will query DNS through Pi-hole and Unbound.

## Complete

Everything is now set up for ad blocking and private DNS resolution. Just remember to update blocklists regularly and keep the Docker image up to date.
