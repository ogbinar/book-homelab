# Chapter 6: Your First Network

## What You'll Build
A clear understanding of your home network, a configured static IP for your server, and the ability to access your homelab services from any device on your network. You'll understand IPs, DNS, ports, and routers — the plumbing that makes everything work.

## How Long It Takes
2 hours (including reading, configuration, and troubleshooting)

## What You Need
- Your server from Chapter 2 (running Ubuntu Server with Docker)
- Access to your router's admin page
- At least one other device on the same network (phone or laptop)

---

## The Story

Imagine your house has 10 rooms. Each room has a door with a number. If you want to deliver a package to Room 5, you go to Door 5. Simple.

Now imagine your house has 10 rooms, but the doors have no numbers. You have to knock on every door until you find the right one. That's what happens when devices on a network don't have proper addresses.

**Networking is just plumbing for the internet.** You're the plumber now. And like any plumber, you need to understand: where do pipes go? Where are the valves? What happens when something leaks?

Your home network probably works right now — your phone gets internet, your laptop streams Netflix, your smart TV plays YouTube. But do you know how? Do you know what happens when you type a URL and hit Enter? *Kung naka-dependé ka sa internet every day, worth it 'yang effort na ma-intindihan mo kung paano ito gumagana.*

Sa maraming Filipino homes, one router feeds a lot of devices, and nobody thinks about the wiring until something slows down or disappears. Once you understand the layout, the whole house makes more sense.

Let's find out.

As you learn this, keep a light stakeholder lens in mind: some things are just for you, some are shared with the family, and some are never meant to leave the house at all.

---

## Why This Matters

Networking is the #1 skill that separates beginners from intermediate homelabbers. Once you understand it, you can:
- Access your services from any device
- Set up custom domains for your homelab
- Troubleshoot connectivity issues
- Secure your network properly
- Connect multiple servers and devices

Without networking knowledge, you're just running containers in the dark. With it, you're an architect. *Hindi ka na lang "yung tipong nagse-setup ng Docker pero hindi alam kung bakit hindi connected ang services."*

That same network thinking is what later helps you separate private services, family-friendly services, and anything that might eventually face the public internet.

---

## 🟢 Quick Start

### Step 1: Understand Your Network

Every device on your network has an **IP address** — a unique number that identifies it.

**Find your network's IP range:**
```bash
# On your server, find your IP address
ip addr show

# Or use the shorter version
ip a
```

**Expected output:**
```
eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
    inet 192.168.1.100/24 brd 192.168.1.255 scope global eth0
```

The `inet 192.168.1.100` is your server's IP address.
The `/24` means your network is `192.168.1.0/24` — which means devices can have IPs from `192.168.1.1` to `192.168.1.254`.

**📢 Jargon Alert:** "IP address" (Internet Protocol address) — Like a house address, but for devices on a network. Every device needs one. `192.168.1.100` means: Network 192.168.1, Device 100.

**📢 Jargon Alert:** "/24" (CIDR notation) — This is a subnet mask. `/24` means the first 24 bits (3 bytes) are the network part. So `192.168.1.x` where x can be 1-254. In plain terms: your network has 254 possible devices.

### Step 2: Find Your Router (Your Gateway)

Your router is the device that connects your home network to the internet. It's also the device that assigns IP addresses to your devices (through DHCP).

```bash
# Find your default gateway (your router's IP)
ip route | grep default
```

**Expected output:**
```
default via 192.168.1.1 dev eth0
```

Your router is at `192.168.1.1`.

**Open your router's admin page:**
1. Open a web browser on your computer
2. Go to `http://192.168.1.1` (or whatever your gateway IP is)
3. Log in with your router's admin credentials

**📢 Jargon Alert:** "Default gateway" — The device that routes traffic between your local network and the internet. Think of it as the doorway between your home and the outside world.

### Step 3: Find All Devices on Your Network

```bash
# Scan your network for active devices
sudo apt install nmap
sudo nmap -sn 192.168.1.0/24
```

This will list all devices that responded on your network. You'll see your phone, laptop, smart TV, router, and server — all with their IP addresses and sometimes their hostnames.

**Expected output:**
```
Nmap scan report for 192.168.1.1
Host is up (0.002s latency).
Nmap scan report for 192.168.1.100
Host is up (0.001s latency).
Nmap scan report for 192.168.1.105
Host is up.
```

### Step 4: Set a Static IP for Your Server

Right now, your server probably gets its IP from your router's DHCP server. This means the IP could change if the router reboots or if there's a DHCP conflict. For a server, we want a **static IP** — one that never changes.

**There are two ways to do this:**

**Option A: Reserve the IP in your router (Recommended)**

1. Log into your router's admin page (`http://192.168.1.1`)
2. Find the DHCP settings (usually under "LAN" or "Network" settings)
3. Look for "DHCP Reservation" or "Static DHCP" or "Address Reservation"
4. Add a reservation for your server's MAC address to the IP you want (e.g., `192.168.1.100`)
5. Save and apply

Your server's MAC address:
```bash
# Find your server's MAC address
ip link show eth0
```

Look for `link/ether XX:XX:XX:XX:XX:XX` — that's your MAC address.

**Option B: Set a static IP on the server itself**

```bash
# Edit the Netplan configuration
sudo nano /etc/netplan/00-installer-config.yaml
```

Replace the content with:
```yaml
network:
  ethernets:
    eth0:
      dhcp4: no
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
          - 192.168.1.1
          - 8.8.8.8
  version: 2
```

Apply the changes:
```bash
sudo netplan apply
```

> **⚠️ The Pitfall:** When setting a static IP, make sure the IP you choose is outside your router's DHCP range. For example, if your router hands out IPs from `.50` to `.150`, don't set your server to `.100` — pick something like `.10` or `.200`.

### Step 5: Access Your Services From Another Device

This is the moment of truth. Open a web browser on your phone or another laptop and go to:

```
http://192.168.1.100:3001
```

(Uptime Kuma, or replace with your service's port)

**You should see your service.**

If you see it: congratulations, your network is working. *Nagaling tayo — you just proved that your server and your phone can talk to each other.*
If you don't see it: don't panic. Let's troubleshoot.

**Troubleshooting checklist:**
1. Is the server running? (`docker ps`)
2. Can you ping the server from your phone? (Most phones can't ping, but your laptop can: `ping 192.168.1.100`)
3. Is the port correct? Check with `docker ps` to see which ports are mapped
4. Is the firewall blocking it? (Ubuntu's UFW might be on — check with `sudo ufw status`)

### Step 6: Make It Pretty — Custom Hostnames

Having to remember `http://192.168.1.100:3001` is tedious. Let's use hostnames instead.

On your server, add entries to the local hosts file:

```bash
# Edit the hosts file
sudo nano /etc/hosts
```

Add these lines at the bottom:
```
192.168.1.100   homelab.local
192.168.1.100   kuma.homelab.local
192.168.1.100   vault.homelab.local
192.168.1.100   nextcloud.homelab.local
192.168.1.100   pihole.homelab.local
192.168.1.100   jellyfin.homelab.local
```

Now from your server, you can access services by name:
```bash
# Instead of this:
curl http://192.168.1.100:3001

# You can do this:
curl http://kuma.homelab.local:3001
```

> **💡 Quick Win:** For devices on your network to use these hostnames too, you'll need a local DNS server. Pi-hole (Chapter 4, Option C) can handle this automatically. After setting up Pi-hole, devices using it as DNS can resolve `.local` hostnames.

---

## 🔵 The Why

### How Networking Actually Works

When you type `http://kuma.homelab.local:3001` and hit Enter, here's what happens:

```
1. Browser looks up "kuma.homelab.local" in the hosts file → 192.168.1.100
2. Browser sends a request to 192.168.1.100 on port 3001
3. The request travels through your network cable/WiFi to the server
4. The server's network card receives it
5. The kernel routes it to Docker (because port 3001 is mapped to a container)
6. The container (Uptime Kuma) receives it and responds
7. The response travels back through the same path
8. Your browser displays the page
```

**All of this happens in under 100 milliseconds.**

### What Is a Port?

A port is like an apartment number in a building. Your IP address is the building address. The port is which apartment (service) you want to reach.

```
192.168.1.100:3001  = Building 192.168.1.100, Apartment 3001
192.168.1.100:8080  = Building 192.168.1.100, Apartment 8080
```

**Common ports you'll use:**
| Port | Service |
|---|---|
| 80 | HTTP (web) |
| 443 | HTTPS (secure web) |
| 22 | SSH |
| 3001 | Uptime Kuma |
| 8080 | Vaultwarden / Alternative HTTP |
| 8081 | Nextcloud |
| 8096 | Jellyfin |
| 53 | DNS (Pi-hole) |

> **📢 Jargon Alert:** "Port" — A numbered endpoint for network communication. Think of it like a door number in a building. The IP address is the building; the port is which room you want to enter.

### IP Address Types

| Type | Example | Use Case |
|---|---|---|
| **Private (LAN)** | 192.168.1.x, 10.x.x.x, 172.16.x.x | Internal network only — your homelab |
| **Public (WAN)** | Assigned by your ISP — visible on the internet | What the outside world sees |
| **Loopback** | 127.0.0.1 | Your own computer only ("localhost") |

**📢 Jargon Alert:** "Private IP" — An IP address that only works inside your home network. Devices with private IPs can't be reached from the internet directly. All your homelab devices use private IPs.

**📢 Jargon Alert:** "Public IP" — The address your router presents to the internet. Your ISP assigns this. If you want to access your homelab from outside your home (from a coffee shop, for example), you need to understand public IPs and port forwarding.

### DHCP — The Address Dispatcher

Your router's DHCP server automatically assigns IP addresses to devices that join your network. When your phone connects to WiFi, the router says: "You can be 192.168.1.105. Use it."

DHCP leases typically last 24 hours. After that, the device requests a renewal. This is why IPs can change — unless you set a reservation (Step 4).

---

## 🟣 Deep Dive

### Network Architecture of a Homelab

```
                    ┌──────────────┐
                    │   Internet   │
                    └──────┬───────┘
                           │
                    ┌──────▼───────┐
                    │   Router     │
                    │  (192.168.1.1)│
                    │  DHCP + NAT  │
                    └──────┬───────┘
                           │
                    ┌──────▼───────┐
                    │   Switch     │
                    │  (or WiFi)   │
                    └──────┬───────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
        ┌─────▼─────┐ ┌───▼────┐ ┌────▼─────┐
        │  Server   │ │ Phone  │ │  TV      │
        │  .100     │ │ .105   │ │  .110    │
        │  Docker   │ │        │ │          │
        └───────────┘ └────────┘ └──────────┘
```

### Common PH Router Models and Their Settings

**PLDT HomeFiber:**
- Admin page: `192.168.254.254`
- Default credentials: Check the sticker on your router
- DHCP settings: LAN → DHCP Server → Address Reservation

**Globe At Home:**
- Admin page: `192.168.254.254` or `10.10.1.1`
- Default credentials: Check the sticker
- DHCP settings: LAN → DHCP → Static DHCP

**Converge:**
- Admin page: Usually `192.168.1.1` or `192.168.8.1`
- Default credentials: Check the sticker
- DHCP settings: Vary by router model

> **📢 Jargon Alert:** "NAT" (Network Address Translation) — The process by which your router allows multiple devices on your home network to share a single public IP address. When your phone requests google.com, NAT replaces your private IP (192.168.1.105) with your public IP, and tracks which request belongs to which device.

### Port Forwarding — Accessing From Outside

If you want to access your homelab from outside your home network (e.g., from work), you need **port forwarding** — telling your router to send incoming traffic on specific ports to your server.

⚠️ **Security warning:** Port forwarding exposes your services to the internet. Only do this if you understand the security implications. Chapter 12 covers security in depth. For now, stick to local access.

**General process (varies by router):**
1. Log into your router
2. Find "Port Forwarding" or "Virtual Server"
3. Add a rule:
   - External port: 3001
   - Internal IP: 192.168.1.100 (your server)
   - Internal port: 3001
   - Protocol: TCP

---

## 💼 Career Boost

**What you learned that employers care about:**
- IP addressing and subnetting
- DHCP and DNS fundamentals
- Network troubleshooting methodology
- Port forwarding and NAT concepts
- Network security basics (firewalls)

**Interview talking point:**
> *"I designed and configured a home network with static IP assignments, custom DNS resolution using Pi-hole, and service discovery via `.local` hostnames. I understand DHCP, NAT, port forwarding, and subnetting fundamentals, and can troubleshoot connectivity issues across multi-device networks."*

---

## 🇵🇭 PH Context

### Common PH ISP Router Models

| ISP | Common Router Models | Admin Page |
|---|---|---|
| PLDT | NX1000, NX2000, SkyBroadband | 192.168.254.254 |
| Globe | AXIS RT55U, Huawei HG8245H | 192.168.254.254 |
| Converge | ZTE F660, Huawei | Varies |
| DSL | D-Link, TP-Link | 192.168.1.1 |

### CGNAT — When Your ISP Doesn't Give You a Public IP

Many Philippine ISPs now use **CGNAT** (Carrier-Grade NAT), meaning you don't have a public IP address — you share one with other customers. This makes port forwarding impossible.

**How to check if you have a public IP:**
1. Find your router's WAN IP (check router admin page)
2. Go to [whatismyipaddress.com](https://whatismyipaddress.com) from a device on your network
3. If the two IPs match, you have a public IP
4. If they don't match, you're behind CGNAT

**Workarounds for CGNAT:**
- **Cloudflare Tunnel** (Chapter 7) — Creates an outgoing connection that bypasses CGNAT
- **Tailscale/ZeroTier** — Mesh VPN that works through NAT
- **Call your ISP** — Some will give you a public IP for an extra fee (₱300-₱500/month)

### WiFi vs Ethernet for Homelab

| Factor | Ethernet | WiFi |
|---|---|---|
| Speed | 100Mbps-10Gbps | 300Mbps-2.4Gbps |
| Latency | Low, consistent | Variable |
| Reliability | High | Can drop |
| Convenience | Cable management | No cables |

**Recommendation:** Always use Ethernet for your server. WiFi is fine for client devices (phones, laptops) but not for a server that needs to be always-on and always-connected.

> **💸 Lean Path:** You don't need a managed switch for a homelab. A ₱500-₱1,000 unmanaged switch from Shopee or Lazada (TP-Link, D-Link) handles everything a beginner needs. Managed switches (₱3,000+) add VLAN support — useful later, not now.

---

## Stress Test

Now let's prove your network setup works:

1. **Change your server's IP** — temporarily change it to something else (e.g., `192.168.1.101`), then change it back. This proves you understand how to reconfigure a static IP without panicking.
2. **Disconnect your Ethernet cable** and plug it into a different port on your router. Wait 30 seconds. Can you still SSH in? If yes, your static IP is working.
3. **Try to access your server from a different network.** Connect your phone to mobile data (not WiFi). Try to reach `192.168.1.100`. It should fail — that's correct. Private IPs are not routable outside your network. This proves you understand the difference between LAN and WAN.
4. **Clear your `/etc/hosts` file** on your computer. Then try to reach `kuma.homelab.local`. It should fail. Add the entry back. This proves you understand how local DNS resolution works.

> **🔥 The Chaos Champion:** Change your router's DHCP range (e.g., from `192.168.1.100-200` to `192.168.1.50-150`). Then reboot your server. If you're using a DHCP reservation (not a static IP on the server itself), your server should get the same IP. If you're using a static IP on the server that falls outside the DHCP range, it should still work — but it's not best practice. This teaches you the difference between DHCP reservation and static IP.

---

## What's Next

You understand your network. Your server has a static IP. You can access your services from any device. In Chapter 7, we'll level up: setting up a reverse proxy so you can access all your services through clean domain names (like `vault.home.local`) with automatic SSL certificates.

**Homework:**
1. Set a static IP or DHCP reservation for your server
2. Scan your network and identify all connected devices
3. Access your services from at least 2 different devices
4. Add custom hostnames in `/etc/hosts`
5. Write down your network diagram (even a simple one)

> **🚀 Turbo:** Run `nmap -sn 192.168.1.0/24` (adjust your subnet) to discover every device on your network. Then try `nmap -A 192.168.1.1` to see what your router is running. Understanding your network topology is half the battle — nmap makes it visible.

---

## Go Deeper

- [Cloudflare's Networking Glossary](https://developers.cloudflare.com/learning-paths/networking/) — Excellent networking explanations
- [Nmap Documentation](https://nmap.org/book/) — Network scanning tool reference
- [Linux Netplan Documentation](https://netplan.readthedocs.io/) — Ubuntu network configuration
- [RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918) — The original spec for private IP ranges
