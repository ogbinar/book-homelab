# Chapter 7: The Reverse Proxy

## What You'll Build
A reverse proxy (Traefik or Caddy) that lets you access all your homelab services through clean, friendly URLs with automatic SSL certificates. `vault.home.local`, `kuma.home.local`, `nextcloud.home.local` — all working through a single port.

## How Long It Takes
2 hours (including reading and configuration)

## What You Need
- Your server from Chapter 2 with Docker and services running
- A domain name (optional — we'll cover both domain and no-domain approaches)
- At least 2 services running (from Chapter 4)

---

## The Story

You've got multiple services running on your homelab. To access them, you've been typing things like:

```
http://192.168.1.100:3001   # Uptime Kuma
http://192.168.1.100:8080   # Vaultwarden
http://192.168.1.100:8081   # Nextcloud
http://192.168.1.100:8096   # Jellyfin
```

That's... not great. Every service needs a different port number. You have to remember which port goes with which service. And if you ever want to access your homelab from outside your home, you need to figure out port forwarding for each one.

Enter the **reverse proxy** — the "concierge" of your homelab.

A reverse proxy sits in front of all your services and directs traffic to the right one based on the URL you type. It's like a building concierge: you tell them which apartment you want to visit, and they direct you there. You don't need to know the building's layout.

With a reverse proxy:
```
http://vault.home.local          → Vaultwarden
http://kuma.home.local           → Uptime Kuma
http://nextcloud.home.local      → Nextcloud
http://jellyfin.home.local       → Jellyfin
```

One port (80 or 443). Clean URLs. Automatic SSL. Magic.

---

## Why This Matters

A reverse proxy is the single most impactful networking upgrade you can make to your homelab. It:

1. **Simplifies access** — One port, many services
2. **Adds SSL automatically** — Encrypted connections for free
3. **Enables external access** — Access your homelab from anywhere (with proper setup)
4. **Centralizes configuration** — All routing in one place
5. **Prepares you for Kubernetes** — Ingress controllers work the same way

> **📢 Jargon Alert:** "Reverse proxy" — A server that sits in front of other servers and forwards client requests to the appropriate backend server. A regular proxy forwards on behalf of the client; a reverse proxy forwards on behalf of the server. Think of it as a receptionist: you talk to the receptionist, and they route you to the right department.

---

## 🟢 Quick Start

We'll use **Caddy** as our reverse proxy. Here's why:

| Feature | Traefik | Caddy |
|---|---|---|
| Auto-SSL | Yes | Yes |
| Configuration | YAML files | Caddyfile (simpler) |
| Docker integration | Excellent | Good |
| Learning curve | Medium | Low |
| Best for | Kubernetes users | Everyone else |

Caddy's configuration is so simple it almost reads like English:

```
vault.home.local {
    reverse_proxy uptime-kuma:80
}
```

That's it. One service, one line.

### Step 1: Create a Docker Compose File for Caddy

Create a file called `docker-compose.yml` in a new directory:

```bash
mkdir -p ~/reverse-proxy && cd ~/reverse-proxy
nano docker-compose.yml
```

```yaml
# docker-compose.yml
version: "3.8"

services:
  caddy:
    image: caddy:latest
    container_name: caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./caddy-data:/data
      - ./caddy-config:/config
    network_mode: host
```

> **Note:** We use `network_mode: host` because Caddy needs to bind to ports 80 and 443 directly. This gives it the best performance and simplicity.

### Step 2: Create the Caddyfile

```bash
mkdir -p ~/reverse-proxy/Caddyfile
nano ~/reverse-proxy/Caddyfile
```

Start with a basic configuration:

```
# Caddyfile — Your homelab reverse proxy

# Internal services (no SSL needed for local access)
{
    admin off
    auto_https off
}

# Replace these with your actual service container names and ports.
# Remove routes for services you haven't deployed — this Caddyfile is a template.
# Uncomment and customize the ones you need:

# Uptime Kuma
# kuma.homelab.local {
#     reverse_proxy uptime-kuma:3001
# }

# Vaultwarden
vault.homelab.local {
    reverse_proxy vaultwarden:80
}

# Nextcloud
nextcloud.homelab.local {
    reverse_proxy nextcloud:80
}

# Jellyfin
# jellyfin.homelab.local {
#     reverse_proxy jellyfin:8096
# }

# Pi-hole
# pihole.homelab.local {
#     reverse_proxy pihole:80
# }
```

### Step 3: Update Your Hosts File

For local DNS resolution, add entries to your computer's hosts file:

```bash
# On your server (and optionally on other devices)
sudo nano /etc/hosts
```

Add:
```
192.168.1.100   kuma.homelab.local
192.168.1.100   vault.homelab.local
192.168.1.100   nextcloud.homelab.local
192.168.1.100   jellyfin.homelab.local
```

### Step 4: Start Caddy

```bash
cd ~/reverse-proxy
docker compose up -d
```

### Step 5: Test It

Open your browser and go to:
```
http://kuma.homelab.local
http://vault.homelab.local
http://nextcloud.homelab.local
http://jellyfin.homelab.local
```

If you see your services, it works. 🎉

### Step 6: Add Automatic SSL (Optional — Requires a Domain)

If you have a domain name (even a cheap one, ~₱500/year from Namecheap or Cloudflare), Caddy will automatically get and renew Let's Encrypt SSL certificates.

1. Point your domain's DNS to your server's public IP
2. Update the Caddyfile to use `https://` (Caddy handles the rest):

```
kuma.homelab.local {
    reverse_proxy uptime-kuma:3001
}
```

Caddy will automatically:
- Get an SSL certificate from Let's Encrypt
- Renew it before it expires
- Redirect HTTP to HTTPS

> **💡 Quick Win:** Even without a domain, the reverse proxy is useful. It gives you clean internal URLs and makes adding new services trivial — just add one line to the Caddyfile and restart Caddy.

---

## 🔵 The Why

### How a Reverse Proxy Works

```
Client                  Reverse Proxy              Services
  │                         │                         │
  │  http://vault.home.loc  │                         │
  │────────────────────────▶│                         │
  │                         │                         │
  │                         │  Route: / → vaultwarden │
  │                         │────────────────────────▶│
  │                         │                         │
  │                         │  Response               │
  │                         │◀────────────────────────│
  │                         │                         │
  │  HTML page              │                         │
  │◀────────────────────────│                         │
```

The reverse proxy:
1. Receives the client's request
2. Looks at the hostname (`vault.homelab.local`)
3. Forwards the request to the correct backend service (vaultwarden:80)
4. Returns the response to the client
5. The client never knows there was a proxy — it just sees `vault.homelab.local`

### Why Caddy Over Traefik?

Both are excellent tools. Traefik is more popular in the Kubernetes world. Caddy is simpler and has better default behavior. For a homelab that may or may not include Kubernetes later, Caddy is the easier starting point.

> **⚠️ Watch Out:** If you use `network_mode: host` with Caddy, the `reverse_proxy` directives need to use the actual container hostnames OR the server's IP address. Docker's internal DNS may not resolve container names in host mode. If you encounter resolution issues, use the server's IP:
> ```
> vault.homelab.local {
>     reverse_proxy 192.168.1.100:8080
> }
> ```

---

## 🟣 Deep Dive

### Caddy Docker Labels (Advanced)

Instead of manually writing the Caddyfile, Caddy can auto-discover Docker containers using labels. This is more "magical" but saves configuration files.

Add these labels to your service's docker-compose.yml:

```yaml
services:
  vaultwarden:
    image: vaultwarden/server:latest
    labels:
      - "caddy=vault.homelab.local"
      - "caddy.reverse_proxy=@* {{.Upstream}}:80"
```

Then Caddy will automatically create routes for any container with a `caddy.*` label.

### SSL Certificate Management

Caddy uses the ACME protocol to communicate with Let's Encrypt:

```
Caddy ──▶ Let's Encrypt CA ──▶ Certificate
  │                                │
  │◀── Certificate Signed ─────────┘
  │
  │◀── Certificate Renewal (auto, 30 days before expiry)
```

Caddy handles the entire lifecycle:
- Certificate issuance
- Automatic renewal
- OCSP stapling
- HTTP-01 or DNS-01 challenge methods

### Adding a New Service

With a reverse proxy, adding a new service is a 3-step process:

1. Deploy the container (Chapter 3)
2. Add one line to the Caddyfile
3. Restart Caddy: `docker compose restart caddy` (or `docker compose up -d --force-recreate caddy`)

That's it. The reverse proxy handles the rest.

---

## Stress Test

Now let's prove your reverse proxy is solid:

1. **Delete one service container** (e.g., `docker rm -f uptime-kuma`). Then add its route to the Caddyfile. When you restart Caddy, the route should be there but the service won't respond — because the container is gone. Recreate the container. The route works again. This proves the proxy is separate from the services.
2. **Remove a route from the Caddyfile** and restart Caddy. The service should be inaccessible through the proxy URL. Add it back. This proves you control routing, not Docker.
3. **Change the port of a service** (e.g., from `3001` to `3002`). Update the Caddyfile to match. If the route doesn't update, it breaks. This teaches you the connection between services and proxy configuration.
4. **Simulate a domain outage** — take one of your `/etc/hosts` entries and comment it out. Try to reach the service. It should fail. Uncomment it. This proves you understand local DNS resolution.

> **🔥 The Chaos Champion:** Add a route for a service that doesn't exist. Something like `ghost.homelab.local { reverse_proxy ghost:80 }`. Restart Caddy. Caddy should start fine — the route is just a configuration. The service won't respond because the container doesn't exist. Now deploy the actual container. The route works. This proves the reverse proxy is just a router — it doesn't care if the backend exists or not. You control the routing, not the proxy.

---

## 💼 Career Boost

**What you learned that employers care about:**
- Reverse proxy configuration (Caddy)
- SSL/TLS certificate management
- DNS configuration and resolution
- Service routing and load balancing concepts
- Domain name management

**Interview talking point:**
> *"I deployed a reverse proxy (Caddy) with automatic SSL/TLS management, routing multiple internal services through clean URLs. This reduced the external attack surface by consolidating access through a single port (443) with centralized security."*

---

## 🇵🇭 PH Context

### Domain Names in the Philippines

Registering a domain in the PH costs ₱500-₱1,500/year:
- **Namecheap** — .com domain for ~$10/year (~₱560), ships worldwide
- **Cloudflare** — .com domain at wholesale price (~$9.77/year), no markup
- **DotPH** — Local PH registrar, supports .ph domains (₱1,000-₱2,000/year)

### CGNAT Considerations

Most Philippine ISPs (PLDT, Globe) use CGNAT (Carrier-Grade NAT), meaning you don't have a public IP. This affects reverse proxy setup:

| Scenario | Can You Access Remotely? | Solution |
|---|---|---|
| No CGNAT + domain | ✅ Yes | Caddy auto-SSL works |
| CGNAT + domain | ❌ No (from WAN) | Use Cloudflare Tunnel (free) |
| CGNAT + local domain | ✅ Yes (LAN only) | Works fine for home use |
| Public IP + domain | ✅ Yes | Full Caddy auto-SSL |

**Cloudflare Tunnel** (free) creates an outgoing connection from your server to Cloudflare, bypassing CGNAT. It's the best solution for PH homelabbers who want remote access.

### Upload Speed Reality

Most PH home internet plans have asymmetric speeds:
- **PLDT Home Fiber:** 150Mbps down / 50Mbps up
- **Globe:** 100-300Mbps down / 20-50Mbps up
- **DITO:** 150Mbps down / 30Mbps up

Your reverse proxy performance for REMOTE access depends on upload speed, not download. If you're only accessing your homelab locally (on your home network), upload speed doesn't matter at all.

---

## What's Next

With a reverse proxy, your homelab is organized and accessible. In Chapter 8, we'll learn how to organize multiple services into a single, maintainable Docker Compose stack — the "one system" approach that scales.

**Homework:**
1. Deploy Caddy as your reverse proxy
2. Add routes for all your services
3. Test each service through the proxy
4. Add a new service and route it through the proxy

---

## Go Deeper

- [Caddy Documentation](https://caddyserver.com/docs/) — Excellent, interactive docs
- [Caddyfile Directives Reference](https://caddyserver.com/docs/caddyfile/directives) — Full directive reference
- [Let's Encrypt](https://letsencrypt.org/) — Free SSL certificates
- [Traefik Documentation](https://doc.traefik.io/traefik/) — Alternative reverse proxy
