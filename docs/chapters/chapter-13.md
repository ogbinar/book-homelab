# Chapter 13: Remote Access

## What You'll Build
The ability to access your homelab from anywhere — your phone on mobile data, a coffee shop, or your commute. You'll set up Tailscale for encrypted mesh networking and Cloudflare Tunnel for public-facing services, both working through CGNAT.

## How Long It Takes
1.5 hours (including reading and both setups)

## What You Need
- Your homelab stack running (Chapters 1-12)
- A phone or laptop outside your home network (for testing)
- A Cloudflare account (free)

---

## The Story

You're at a coffee shop. Your partner texts: *"Can you check if the backup from last night worked?"*

You open your phone, go to `kuma.homelab.local`... and it doesn't load. Of course it doesn't. You're not on your home network. Private IPs don't work outside your house. That's how networking works.

But you built a homelab. You should be able to check on it from anywhere. Not because you need to — but because you can.

This chapter is about breaking the leash. Your homelab lives in your house, but you should be able to access it from anywhere in the world. And the best part? You don't need a public IP. You don't need to mess with your router. And you don't need to expose anything to the internet unsafely.

There's a Filipino saying: *"Bahay kung saan ka pinagkanan, doon ka rin makikita."* The house where you were born is where you'll always be found. Your homelab is the same — it has a home, but you can reach it from anywhere.

---

## Why This Matters

Remote access is what separates a lab that sits at home from a lab that **works with you everywhere**.

With remote access, you can:
- **Check monitoring** when you're away from home
- **Access your files** from your phone on the go
- **Troubleshoot issues** without rushing home
- **Show off your setup** to friends who ask *"Wait, I can do what from my phone?!"*

And unlike port forwarding (which exposes your homelab directly to the internet), these methods create **encrypted tunnels** — your traffic is protected from end to end.

> **📢 Jargon Alert:** "CGNAT" (Carrier-Grade NAT) — When your ISP doesn't give you a public IP address, you share one with other customers. Your router's WAN IP is a private address. This means port forwarding doesn't work. Most Philippine ISPs (PLDT, Globe, DITO) use CGNAT by default.

---

## 🟢 Quick Start

### Step 1: Tailscale — Your Homelab, Everywhere (Easiest)

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
- Download the Tailscale app from the App Store or Google Play
- Log in with the same account
- Toggle the switch to "On"

**Access your homelab from your phone:**
```
http://kuma.homelab.local:3001
```

Wait — that won't work yet. On Tailscale, your server gets a `.tailnet` address like `100.x.y.z`. But Tailscale has a built-in DNS feature: any hostname that resolves on your server will also resolve on every device in your tailnet.

So if `kuma.homelab.local` resolves on your server (via `/etc/hosts` or Pi-hole), it will resolve on your phone too.

**Test it from your phone:**
1. Make sure you're on mobile data (not WiFi)
2. Open your browser
3. Go to `http://[100.x.y.z]:3001` (use your server's Tailscale IP)
4. You should see Uptime Kuma

> **💡 Quick Win:** Tailscale is free for personal use (up to 100 devices, 3 users). Install it now and you have remote access in 5 minutes. You can set up Cloudflare Tunnel later for more advanced routing.

### Step 2: Cloudflare Tunnel — Public-Facing Services (Advanced)

Tailscale is great for personal access. But what if you want to share a service with someone else? Or access it from a device you don't want to install Tailscale on?

[Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) creates an outgoing connection from your server to Cloudflare's network. No port forwarding needed. No open ports on your router. Your server initiates the connection, so CGNAT doesn't matter.

**Step 2a: Install Cloudflared**
```bash
# Download Cloudflared
sudo mkdir -p -p /etc/cloudflared && \
curl -L -o /tmp/cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && \
sudo dpkg -i /tmp/cloudflared.deb
```

**Step 2b: Configure the Tunnel**
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

**Step 2c: Create DNS Records**
```bash
# This creates DNS records automatically
sudo cloudflared tunnel route dns homelab kuma.yourdomain.com
sudo cloudflared tunnel route dns homelab vault.yourdomain.com
sudo cloudflared tunnel route dns homelab nextcloud.yourdomain.com
sudo cloudflared tunnel route dns homelab jellyfin.yourdomain.com
```

**Step 2d: Start the Tunnel**
```bash
# Test the tunnel
sudo cloudflared tunnel run

# If it works, set it up as a systemd service
sudo cloudflared service install

# Start and enable
sudo systemctl start cloudflared
sudo systemctl enable cloudflared
```

**Step 2e: Verify**
Open any browser (even on a device without Tailscale) and go to `https://kuma.yourdomain.com`. You should see your homelab service.

> **⚠️ The Pitfall:** Cloudflare Tunnel routes traffic over HTTPS by default. If your services don't support HTTPS internally (they don't — they're behind a reverse proxy), Cloudflare handles the TLS termination at the edge. This means your traffic is encrypted from your browser to Cloudflare, and then travels unencrypted over your LAN to your homelab. For home use, this is fine. The sensitive part (browser to Cloudflare) is encrypted.

---

## 🔵 The Why

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

### How Tailscale Works

```
Your Phone ──▶ Tailscale Network ──▶ Your Server
                (encrypted mesh)         │
                                          ▼
                                   Docker Containers
```

Tailscale uses [WireGuard](https://www.wireguard.com/)-encrypted tunnels. Every packet between devices is encrypted. Your ISP can see you're connected to Tailscale, but they can't see what you're doing.

Tailscale also includes MagicDNS — a built-in DNS server for your tailnet. If a hostname resolves on one device, it resolves on all devices. This is why `kuma.homelab.local` works on your phone without any extra configuration.

### How Cloudflare Tunnel Works

```
Your Phone ──▶ Internet ──▶ Cloudflare Edge ──▶ Tunnel ──▶ Your Server
                                                              │
                                                              ▼
                                                       Docker Containers
```

Cloudflare Tunnel works differently. Your server initiates an outgoing, encrypted connection to Cloudflare. Cloudflare listens for requests on your domain and forwards them through that existing connection to your server.

**Key insight:** Your server makes the connection outward. Your router doesn't need any port forwarding rules. CGNAT doesn't matter. This is why Cloudflare Tunnel is the go-to solution for PH homelabbers behind CGNAT.

### Security: Why These Are Better Than Port Forwarding

Port forwarding opens a door on your router directly to your server. Anyone on the internet can knock on that door. If your service has a vulnerability, they're in.

Tailscale and Cloudflare Tunnel are more secure because:

1. **No open ports** — Your router doesn't expose any services
2. **Encryption** — Traffic is encrypted end-to-end (Tailscale) or at the edge (Cloudflare)
3. **Authentication** — Tailscale requires login; Cloudflare works with your service's own auth
4. **No CGNAT headaches** — Both work through carrier-grade NAT

> **⚠️ Watch Out:** Even with a tunnel, your services still need their own authentication. A Cloudflare Tunnel is not a substitute for a password. Make sure every service has strong authentication enabled.

---

## 🟣 Deep Dive

### Advanced Tailscale: Access Control

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

### Cloudflare Tunnel: Subdomains and Multiple Services

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

## 💼 Career Boost

**What you learned that employers care about:**
- Remote network access and mesh networking
- Zero-trust network access (ZTNA) concepts
- Cloud tunneling and edge computing
- CGNAT workarounds and enterprise networking
- DNS configuration and management

**Interview talking point:**
> *"I implemented secure remote access to my homelab infrastructure using Tailscale for private mesh networking and Cloudflare Tunnel for public-facing services. Both solutions work through CGNAT without port forwarding, providing encrypted access from any location with zero open ports on my network."*

---

## 🇵🇭 PH Context

### CGNAT Is the Default, Not the Exception

Most Philippine ISPs use CGNAT:

| ISP | CGNAT Status | Workaround |
|---|---|---|
| **PLDT** | Yes (most plans) | Cloudflare Tunnel or Tailscale |
| **Globe** | Yes (most plans) | Cloudflare Tunnel or Tailscale |
| **DITO** | Yes (most plans) | Cloudflare Tunnel or Tailscale |
| **Converge** | Sometimes | Depends on your area |

**Bottom line:** Don't waste time trying to get port forwarding to work. Use tunnels. They're easier and more secure.

### Calling Your ISP

Some ISPs will give you a public IP for an extra fee:
- **PLDT:** ~₱300-₱500/month for static IP
- **Converge:** Sometimes free on request
- **Globe:** Rarely available

**Is it worth it?** For a homelab, no. Tunnels are free, easier, and more secure. A public IP is only worth it if you're running services that need direct TCP/UDP access (like a game server).

### Upload Speed Reality

Remote access performance depends on your upload speed:
- **PLDT Home Fiber:** ~50Mbps upload
- **Globe:** ~20-50Mbps upload
- **DITO:** ~30Mbps upload

For checking dashboards and accessing files, this is more than enough. For streaming media remotely, you might want to limit quality to 720p.

---

## Stress Test

Now let's prove your remote access works:

1. **Kill your home WiFi** — Turn off your router's WiFi. Connect your phone to mobile data. Access your homelab via Tailscale. It should work — your phone is on a completely different network.

2. **Share with a friend** — Give your Cloudflare Tunnel URL to a friend (or use a different device). They should be able to access your service without installing anything. This proves the tunnel is public.

3. **Remove a DNS record** — In the Cloudflare dashboard, delete the DNS record for `kuma.yourdomain.com`. Wait 60 seconds. Try to access it. It should fail. Add it back. It works again. This proves DNS controls tunnel routing.

4. **Disconnect Tailscale** — Turn off Tailscale on your phone. Try to access `kuma.homelab.local`. It should fail. Turn Tailscale back on. It works again. This proves Tailscale is what provides the remote DNS resolution.

> **🔥 The Chaos Champion:** Set up a Cloudflare Tunnel for a service that requires authentication (like Vaultwarden). Then ask a friend to try to access it without logging in. They should see the login page. Now log in and show them your passwords. This proves that the tunnel provides transport security, but your service's own auth is the real gatekeeper. Tunnel + auth = defense in depth.

---

## What's Next

You can now access your homelab from anywhere in the world. Securely. Without port forwarding. Without a public IP.

In Chapter 14, we'll document everything you've built and turn it into a professional portfolio that shows employers: **here's what I can do.**

**Homework:**
1. Install Tailscale on your server and phone
2. Access your homelab from mobile data
3. (Optional) Set up Cloudflare Tunnel for one service
4. Test remote access from a device outside your home
5. Take a screenshot of your homelab accessible from your phone — you'll want this for your portfolio (Chapter 14)

---

## Go Deeper

- [Tailscale Documentation](https://tailscale.com/kb/) — Complete reference
- [Cloudflare Tunnel Documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) — Tunnel setup guide
- [WireGuard](https://www.wireguard.com/protocol/) — The encryption protocol Tailscale uses
- [CGNAT Explained](https://en.wikipedia.org/wiki/Carrier-grade_NAT) — Understanding carrier-grade NAT
- [Zero Trust Networking](https://en.wikipedia.org/wiki/Zero_trust_security_model) — Security model behind Tailscale
