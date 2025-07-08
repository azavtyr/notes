# Nginx Proxy Manager Setup

This guide documents how I set up Nginx Proxy Manager with Docker Compose, DuckDNS for dynamic DNS and Let's Encrypt SSL certificates.

## DuckDNS + OpenWrt Local DNS

Update DuckDNS to point to your VM's IP. Then configure OpenWrt to resolve the domain locally:

### Steps:

1. Open OpenWrt: `Network > DHCP and DNS > General`
1. Under Addresses, add: `/yourdomain.duckdns.org/vm-ip`
1. Click `Save & Apply`

This allows local devices to resolve the domain without going to the internet.

## Docker Compose: Nginx Proxy Manager

* Create directory:

```sh
mkdir nginx-proxy-manager/
cd nginx-proxy-manager/
$EDITOR docker-compose.yml
```

* Edit `docker-compose.yml` file:

```yaml
services:
  service:
    image: 'jc21/nginx-proxy-manager:latest'
    container_name: nginx-proxy-manager
    restart: unless-stopped
    networks:
      - proxy
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./nginx-data:/nginx-data
      - ./letsencrypt:/etc/letsencrypt

networks:
  proxy:
    external: true
```

* Create the Docker network:

```sh
docker network create proxy
```

* Start the container:

```sh
docker compose up -d
```

> [!NOTE]
> Reusing the `proxy` network for other apps ensures they can be reverse-proxied by Nginx Proxy Manager without exposing internal ports to the host or external network. This simplifies networking, as all containers on the proxy network can be accessed internally by their container names (e.g., nginx-proxy-manager) and share the same network namespace, reducing configuration overhead and improving security.

## Initial Setup via Web UI

1. Open browser: `http://<vm-ip>:81`
1. Log in with:
    * Email: `admin@example.com`
    * Password: `changeme`
1. Set up the admin account:
    * Change the email to a personal one.
    * Update the password when prompted.

## Set Up Let’s Encrypt SSL via DuckDNS

1. Navigate to `SSL Certificates` > `Add SSL Certificate`
1. Add Domain names:
    * `yourdomain.duckdns.org`
    * `*.yourdomain.duckdns.org`
1. Use DNS Challenge:
    * DNS Provider: DuckDNS
    * API Token: Your DuckDNS token
    * Propagation Time: 30 (increase to 120 if errors occur)
1. Accept TOS and click Save

> [!NOTE]
> * If the DNS challenge fails, verify the DuckDNS token or increase propagation time to 120 seconds.
> * Use the wildcard subdomain (`*.yourdomain.duckdns.org`) to simplify adding more apps (`proxy.yourdomain.duckdns.org`) without needing new SSL certificates.

## Create Proxy Host (Reverse Proxy)

1. Navigate to `Proxy Hosts` > `Add Proxy Host`
1. Fill in:
    * Domain Names: proxy.yourdomain.duckdns.org
    * Scheme: http
    * Forward Hostname/IP: Container name (e.g., nginx-proxy-manager)
    * Forward Port: 81 (or the port your app uses)
1. Optional settings:
    * Enable Websockets support, Block common exploits, or Asset caching as needed.
1. Go to SSL tab:
    * Select the created SSL certificate.
    * Enable Force SSL and HTTP/2 Support.
    * Click Save.

## Test

Visit: `https://yourdomain.duckdns.org`
