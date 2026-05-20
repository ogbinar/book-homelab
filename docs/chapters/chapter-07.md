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

That's... not great. Every service needs a different port number. You have to remember which port goes with which service. And if you ever want to access your homelab from outside your home, you need to figure out port forwarding for each one. *Think about it — you're already tired remembering which port is which. Imagine kung may 10 services ka. Hindi 'yan sustainable.*

Enter the **reverse proxy** — the "concierge" of your homelab.

A reverse proxy sits in front of all your services and directs traffic to the right one based on the URL you type. It's like a building concierge: you tell them which apartment you want to visit, and they direct you there. You don't need to know the building's layout.

With a reverse proxy:
```
http://vault.home.local          → Vaultwarden
http://kuma.home.local           → Uptime Kuma
http://nextcloud.home.local      → Nextcloud
http://jellyfin.home.local       → Jellyfin
```

One port (80 or 443). Clean URLs. Automatic SSL. Magic. *And the best part? Hindi mo na kailangan mag-alala kung anong port ang para sa anong service.*

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

If you see your services, it works. 🎉 *Nandyan na sila — lahat ng services mo, accessible through clean URLs. Parang may personal na web server ka na lang.*

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

> **💸 Lean Path:** You don't need a domain name. The reverse proxy works perfectly with `.local` hostnames and `/etc/hosts` entries for local-only access. Save your ₱500-₱1,500/year for something else. Get a domain when you're ready for remote access — it's the one place where a cheap domain makes real sense.

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

## 🟢 Remote Access Patterns

You've got all your services running behind a reverse proxy on your local network. But what happens when you're not home? You're at a coffee shop, your partner texts: *"Can you check if the backup from last night worked?"* You open your phone, go to `kuma.homelab.local`... and it doesn't load. Of course it doesn't. You're not on your home network. Private IPs don't work outside your house.

This section covers how to access your homelab from anywhere — your phone on mobile data, a coffee shop, or your commute — without opening ports on your router or exposing your lab to the internet unsafely.

### Tailscale — Your Homelab, Everywhere (Easiest)

[Tailscale](https://tailscale.com/) is the fastest way to access your homelab from anywhere. It creates a private, encrypted network between all your devices — your server, your phone, your laptop — even when they're on different networks.

**Install Tailscale on your server:**

```bash
# Download and install Tailscale
curl -fsSL https://tailscale.com/install.sh | sh

# Start Tailscale and authenticate
sudo tailscale up
```

This will give you a URL like `https://tailscale.com/key/abcd1234`. Open it in your browser, log in with Google/GitHub/Microsoft (whichever you prefer), and your server joins the Tailscale network.

**Install Tailscale on your phone:**

1. Download the Tailscale app from the App Store or Google Play
2. Log in with the same account
3. Toggle the switch to "On"

**Access your homelab from your phone:**

On Tailscale, your server gets a `.tailnet` address like `100.x.y.z`. But Tailscale has a built-in DNS feature: any hostname that resolves on your server (via `/etc/hosts` or Pi-hole) will also resolve on every device in your tailnet.

So if `kuma.homelab.local` resolves on your server, it will resolve on your phone too.

**Test it from your phone:**

1. Make sure you're on mobile data (not WiFi)
2. Open your browser
3. Go to `http://100.x.y.z:3001` (use your server's Tailscale IP)
4. You should see Uptime Kuma

> **💡 Quick Win:** Tailscale is free for personal use (up to 100 devices, 3 users). Install it now and you have remote access in 5 minutes. You can set up Cloudflare Tunnel later for more advanced routing.

### Cloudflare Tunnel — Public-Facing Services (Advanced)

Tailscale is great for personal access. But what if you want to share a service with someone else? Or access it from a device you don't want to install Tailscale on?

[Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) creates an outgoing connection from your server to Cloudflare's network. No port forwarding needed. No open ports on your router. Your server initiates the connection, so CGNAT doesn't matter.

**Step 1: Install Cloudflared**

```bash
# Download Cloudflared
sudo mkdir -p -p /etc/cloudflared && \
curl -L -o /tmp/cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && \
sudo dpkg -i /tmp/cloudflared.deb
```

**Step 2: Configure the Tunnel**

```bash
# Create a tunnel (first time only)
sudo cloudflared tunnel create homelab

# This gives you a Tunnel ID. Note it down.
# Example: abcd1234-5678-90ef-ghij-klmnopqrstuv

# Create the config file
sudo nano /etc/cloudflared/config.yml
```

Add this:

```yaml
# /etc/cloudflared/config.yml
tunnel: homelab
credentials-file: /etc/cloudflared/<TUNNEL_ID>.json

protocol: http2

ingress:
  # Uptime Kuma
  - hostname: kuma.yourdomain.com
    service: http://localhost:3001

  # Vaultwarden
  - hostname: vault.yourdomain.com
    service: http://localhost:8080

  # Nextcloud
  - hostname: nextcloud.yourdomain.com
    service: http://localhost:8081

  # Jellyfin
  - hostname: jellyfin.yourdomain.com
    service: http://localhost:8096

  # Catch-all (required)
  - service: http_status:404
```

Replace `yourdomain.com` with your actual domain name. Replace the ports with your actual service ports.

**Step 3: Create DNS Records**

```bash
# This creates DNS records automatically
sudo cloudflared tunnel route dns homelab kuma.yourdomain.com
sudo cloudflared tunnel route dns homelab vault.yourdomain.com
sudo cloudflared tunnel route dns homelab nextcloud.yourdomain.com
sudo cloudflared tunnel route dns homelab jellyfin.yourdomain.com
```

**Step 4: Start the Tunnel**

```bash
# Test the tunnel
sudo cloudflared tunnel run

# If it works, set it up as a systemd service
sudo cloudflared service install

# Start and enable
sudo systemctl start cloudflared
sudo systemctl enable cloudflared
```

**Step 5: Verify**

Open any browser (even on a device without Tailscale) and go to `https://kuma.yourdomain.com`. You should see your homelab service.

> **⚠️ The Pitfall:** Cloudflare Tunnel routes traffic over HTTPS by default. If your services don't support HTTPS internally (they don't — they're behind a reverse proxy), Cloudflare handles the TLS termination at the edge. This means your traffic is encrypted from your browser to Cloudflare, and then travels unencrypted over your LAN to your homelab. For home use, this is fine. The sensitive part (browser to Cloudflare) is encrypted.

### Tailscale vs Cloudflare Tunnel

| Feature | Tailscale | Cloudflare Tunnel |
|---|---|---|
| **Setup time** | 5 minutes | 20 minutes |
| **Access type** | Private (tailnet only) | Public (anyone with the URL) |
| **Needs a domain?** | No | Yes |
| **Works through CGNAT?** | Yes | Yes |
| **Auth** | Tailscale account | Your service's own auth |
| **Best for** | Personal access | Sharing with others |
| **Cost** | Free (personal) | Free (basic) |

### Security: Why These Are Better Than Port Forwarding

Port forwarding opens a door on your router directly to your server. Anyone on the internet can knock on that door. If your service has a vulnerability, they're in.

Tailscale and Cloudflare Tunnel are more secure because:

1. **No open ports** — Your router doesn't expose any services
2. **Encryption** — Traffic is encrypted end-to-end (Tailscale) or at the edge (Cloudflare)
3. **Authentication** — Tailscale requires login; Cloudflare works with your service's own auth
4. **No CGNAT headaches** — Both work through carrier-grade NAT

> **⚠️ Watch Out:** Even with a tunnel, your services still need their own authentication. A Cloudflare Tunnel is not a substitute for a password. Make sure every service has strong authentication enabled.

---

### Advanced: Tailscale Access Control

Tailscale has built-in access controls (ACLs) that let you decide which devices can talk to which services. For personal use, the default is fine. But if you're sharing your tailnet with friends:

```bash
# Create an ACL file
nano ~/tailscale-acl.json
```

```json
{
  "groups": {
    "user:your-email@gmail.com": ["you@tailnet-name"]
  },
  "hosts": {
    "homelab-server": "100.x.y.z"
  },
  "acl": [
    {"action": "accept", "src": ["you@tailnet-name"], "dst": ["homelab-server:*"]}
  ]
}
```

Apply it in the Tailscale dashboard (Settings → Access Controls).

### Advanced: Cloudflare Tunnel — Multiple Services

You can route any subdomain to any service. Here's a more complete example:

```yaml
# /etc/cloudflared/config.yml
ingress:
  # Monitoring
  - hostname: kuma.yourdomain.com
    service: http://localhost:3001

  # Security
  - hostname: vault.yourdomain.com
    service: http://localhost:8080

  # Storage
  - hostname: files.yourdomain.com
    service: http://localhost:8081

  # Media
  - hostname: media.yourdomain.com
    service: http://localhost:8096

  # DNS
  - hostname: dns.yourdomain.com
    service: http://localhost:8082

  # Reverse Proxy
  - hostname: proxy.yourdomain.com
    service: http://localhost:80

  # Catch-all
  - service: http_status:404
```

### Combining Tailscale + Cloudflare Tunnel

Use both! Tailscale for your personal access (fast, private, no domain needed). Cloudflare Tunnel for services you want to share with others or access from devices where you can't install Tailscale.

```
You (Phone)        Friend (Phone)
    │                    │
    ▼                    ▼
Tailscale           Cloudflare
  (private)           (public URL)
    │                    │
    └────▶ Your Server ◀─┘
           (both tunnels)
```

### Custom Domains

For Cloudflare Tunnel, you can use any domain you own. Cheap domains cost ~$10/year (~₱560):

- **Cloudflare Registrar** — Wholesale pricing, no markup
- **Namecheap** — Popular, good support
- **GoDaddy** — Frequently has sales

**DNS setup:** Point your domain's DNS to Cloudflare (free nameservers), then create the tunnel. Cloudflare handles the SSL certificates automatically.

---

### 🇵🇭 PH Context: Remote Access in the Philippines

#### CGNAT Is the Default, Not the Exception

Most Philippine ISPs use CGNAT:

| ISP | CGNAT Status | Workaround |
|---|---|---|
| **PLDT** | Yes (most plans) | Cloudflare Tunnel or Tailscale |
| **Globe** | Yes (most plans) | Cloudflare Tunnel or Tailscale |
| **DITO** | Yes (most plans) | Cloudflare Tunnel or Tailscale |
| **Converge** | Sometimes | Depends on your area |

**Bottom line:** Don't waste time trying to get port forwarding to work. Use tunnels. They're easier and more secure.

#### Calling Your ISP

Some ISPs will give you a public IP for an extra fee:

- **PLDT:** ~₱300-₱500/month for static IP
- **Converge:** Sometimes free on request
- **Globe:** Rarely available

**Is it worth it?** For a homelab, no. Tunnels are free, easier, and more secure. A public IP is only worth it if you're running services that need direct TCP/UDP access (like a game server).

#### Upload Speed Reality

Remote access performance depends on your upload speed:

- **PLDT Home Fiber:** ~50Mbps upload
- **Globe:** ~20-50Mbps upload
- **DITO:** ~30Mbps upload

For checking dashboards and accessing files, this is more than enough. For streaming media remotely, you might want to limit quality to 720p.

---

### 💼 Career Boost

**What you learned that employers care about:**

- Remote network access and mesh networking
- Zero-trust network access (ZTNA) concepts
- Cloud tunneling and edge computing
- CGNAT workarounds and enterprise networking
- DNS configuration and management

**Interview talking point:**

> *"I implemented secure remote access to my homelab infrastructure using Tailscale for private mesh networking and Cloudflare Tunnel for public-facing services. Both solutions work through CGNAT without port forwarding, providing encrypted access from any location with zero open ports on my network."*

---

### Stress Test: Remote Access

Now let's prove your remote access works:

1. **Kill your home WiFi** — Turn off your router's WiFi. Connect your phone to mobile data. Access your homelab via Tailscale. It should work — your phone is on a completely different network.

2. **Share with a friend** — Give your Cloudflare Tunnel URL to a friend (or use a different device). They should be able to access your service without installing anything. This proves the tunnel is public.

3. **Remove a DNS record** — In the Cloudflare dashboard, delete the DNS record for `kuma.yourdomain.com`. Wait 60 seconds. Try to access it. It should fail. Add it back. It works again. This proves DNS controls tunnel routing.

4. **Disconnect Tailscale** — Turn off Tailscale on your phone. Try to access `kuma.homelab.local`. It should fail. Turn Tailscale back on. It works again. This proves Tailscale is what provides the remote DNS resolution.

> **🔥 The Chaos Champion:** Set up a Cloudflare Tunnel for a service that requires authentication (like Vaultwarden). Then ask a friend to try to access it without logging in. They should see the login page. Now log in and show them your passwords. This proves that the tunnel provides transport security, but your service's own auth is the real gatekeeper. Tunnel + auth = defense in depth.

---

## What's Next

With a reverse proxy, your homelab is organized and accessible. You can even access it from anywhere using Tailscale or Cloudflare Tunnel. In Chapter 8, we'll learn how to organize multiple services into a single, maintainable Docker Compose stack — the "one system" approach that scales.

**Homework:**
1. Deploy Caddy as your reverse proxy
2. Add routes for all your services
3. Test each service through the proxy
4. Add a new service and route it through the proxy
5. Install Tailscale on your server and phone
6. Access your homelab from mobile data

---

> **🚀 Turbo:** Want to serve multiple domains from one server? Caddy handles it effortlessly. Add a new block to your Caddyfile for each domain: `vault.yourdomain.com { reverse_proxy vaultwarden:8222 }`. Caddy will automatically provision SSL for each subdomain. You can also use wildcard SSL (`*.yourdomain.com`) if your domain registrar supports DNS challenges — one certificate covers all subdomains.

---

## Go Deeper

- [Caddy Documentation](https://caddyserver.com/docs/) — Excellent, interactive docs
- [Caddyfile Directives Reference](https://caddyserver.com/docs/caddyfile/directives) — Full directive reference
- [Let's Encrypt](https://letsencrypt.org/) — Free SSL certificates
- [Traefik Documentation](https://doc.traefik.io/traefik/) — Alternative reverse proxy
