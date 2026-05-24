# Chapter 4: Something Useful

## What You'll Build
A self-hosted service you'll actually use — chosen from password manager, file storage, ad blocker, or media server. By the end, you'll have a real tool running on your homelab that replaces a subscription or improves your daily life.

## How Long It Takes
2 hours (including reading, deployment, and first-use configuration)

## What You Need
- Your server from Chapter 2 with Docker running (Chapter 3)
- 500MB-1GB of free disk space (depending on which service you choose)
- A web browser on your phone or computer

---

## The Story

You've got a server. You've got Docker. You've got Uptime Kuma watching over everything. But here's the honest truth: if your homelab doesn't do something you actually use, it becomes expensive furniture.

This chapter is about building something useful. Something you'll touch every day. Something that makes you go: *Hindi ko kailangan magbayad ng subscription para sa 'to.*

You get to choose. Pick at least one — we recommend starting with 2-3 so you have enough services for Chapter 8. You can always add more later, but for now, pick a couple and make them good.

---

## Why This Matters

Choosing your first service is about choosing your first win. You want something that:
1. Solves a real problem you have
2. Teaches you something useful
3. You'll actually use every day

The service you pick becomes the proof that self-hosting isn't just a hobby — it's a better way to manage your digital life.

---

## 🟢 Quick Start

### Option A: Vaultwarden — Your Own Password Manager

**Replaces:** LastPass, 1Password, Bitwarden (₱24-₱167/month)

Your passwords should be encrypted and under your control. Vaultwarden is a lightweight, open-source implementation of the Bitwarden API — meaning you can use the official Bitwarden apps on any device, but your data lives on YOUR server.

**Why this is a great first service:**
- You'll use it every single day
- The security benefits are real and immediate
- Setup takes 10 minutes
- Works on any hardware, even a Raspberry Pi

**Deploy:**
```bash
# Create directories
mkdir -p ~/vaultwarden/data && cd ~/vaultwarden

# Run Vaultwarden
docker run -d \
  --name vaultwarden \
  -v ~/vaultwarden/data:/data \
  -p 8080:80 \
  -e ADMIN_TOKEN=$(openssl rand -base64 48) \
  --restart unless-stopped \
  vaultwarden/server:latest
```

Access it at `http://YOUR_SERVER_IP:8080`. Set up your account through the Bitwarden web vault or the Bitwarden app on your phone.

**⚠️ Watch Out:** The `ADMIN_TOKEN` is how you access the admin panel. Save it somewhere safe (your password manager, obviously).

---

### Option B: Nextcloud — Your Own Cloud Storage

**Replaces:** Google Drive, Dropbox, iCloud (₱59-₱167/month for 100GB-2TB)

Nextcloud is a full productivity platform: file storage, calendar, contacts, documents, and more. It's the closest open-source alternative to Google Workspace.

**Why this is a great first service:**
- Replaces multiple subscriptions
- The mobile app is excellent
- You control your data completely
- Collaborative document editing included

**Deploy:**
```bash
# Create directories
mkdir -p ~/nextcloud/{data,config} && cd ~/nextcloud

# Run Nextcloud with SQLite (simpler, fine for personal use)
docker run -d \
  --name nextcloud \
  -v ~/nextcloud/data:/var/www/html/data \
  -v ~/nextcloud/config:/var/www/html/config \
  -p 8081:80 \
  --restart unless-stopped \
  nextcloud:latest
```

Access it at `http://YOUR_SERVER_IP:8081`. Create your admin account on first visit.

> **🚀 Turbo:** For larger setups (50GB+), use Nextcloud with PostgreSQL and Redis instead of SQLite. See "Go Deeper" for the full compose file.

---

### Option C: Pi-hole — Block Ads on Your Entire Network

**Replaces:** No paid service needed — this saves you money by blocking ads and trackers at the network level.

Pi-hole is a DNS-level ad blocker. Once it's running, ALL devices on your network (phones, laptops, tablets, smart TVs) automatically get ads blocked. No browser extensions needed.

**Why this is a great first service:**
- Instant gratification — you'll see the ad count drop immediately
- Set it and forget it
- Surprisingly powerful — blocks thousands of trackers
- Works on the tiniest hardware (even 1GB RAM)

**Deploy:**
```bash
# Create directories
mkdir -p ~/pihole && cd ~/pihole

# Run Pi-hole
docker run -d \
  --name pihole \
  -v ~/pihole/etc-pihole:/etc/pihole \
  -v ~/pihole/etc-dnsmasq.d:/etc/dnsmasq.d \
  -p 53:53/tcp -p 53:53/udp \
  -p 8082:80 \
  -e TZ=Asia/Manila \
  -e WEBPASSWORD=$(openssl rand -base64 16) \
  --restart unless-stopped \
  pihole/pihole:latest
```

Access the admin panel at `http://YOUR_SERVER_IP:8082`.

**To activate Pi-hole:**
1. Go to your router's settings
2. Find the DNS settings
3. Change the DNS server to your homelab's IP address (e.g., `192.168.1.100`)
4. Save and restart your router's DHCP

Now every device on your network uses Pi-hole for DNS. Ads are blocked before they even load.

> **⚠️ The Pitfall:** If you use WiFi on your phone, changing DNS at the router level affects your phone automatically. If you have a device using a custom DNS (like Google DNS 8.8.8.8), you'll need to change that too.

---

### Option D: Jellyfin — Your Own Media Server

**Replaces:** Netflix, Disney+, Amazon Prime Video (₱199-₱349/month per service)

Jellyfin is a free, open-source media server. Put your movies, TV shows, music, and photos on your server and stream them to any device — TV, phone, tablet, laptop.

**Why this is a great first service:**
- You probably have movies/shows you've downloaded but never organized
- The streaming experience is excellent
- Works on any device with a browser
- No subscriptions, ever

**Hardware note:** Basic playback works on any hardware. For hardware transcoding (converting video formats on-the-fly), you'll need a GPU. But for direct playback (streaming files as-is), any CPU works.

**Deploy:**
```bash
# Create directories
mkdir -p ~/jellyfin/{config,media} && cd ~/jellyfin

# Run Jellyfin
docker run -d \
  --name jellyfin \
  -v ~/jellyfin/config:/config \
  -v ~/jellyfin/media:/media \
  -v ~/jellyfin/cache:/cache \
  -p 8096:8096 \
  --restart unless-stopped \
  jellyfin/jellyfin:latest
```

Access it at `http://YOUR_SERVER_IP:8096`. Follow the setup wizard to add your media libraries.

**Where to get media:**
- Your own collection ( DVDs, downloads you already have)
- Public domain content (Internet Archive, Public Domain Torrents)
- Your own photos and home videos (this is actually the most popular use)

> **💸 Lean Path:** Start with your own photos and home videos. That's free, meaningful, and a great way to learn Jellyfin without worrying about "piracy" conversations.

---

## Configure Your Service

Whichever service you chose, take 30 minutes to configure it properly:

1. **Change the default password** (if you haven't already)
2. **Set up two-factor authentication** (if available)
3. **Add some data** — a few files, some passwords, a couple of monitors
4. **Access it from your phone** — most services have mobile apps
5. **Bookmark it** — make it easy to find

> **🧠 The Deep Dive:** The configuration step is where you go from "deployed" to "using." Don't skip it. The effort you put in now determines whether this service becomes part of your daily life or just another container collecting digital dust.

---

## Verify It Works

| Service | How to Verify |
|---|---|
| Vaultwarden | Log in from your phone's Bitwarden app |
| Nextcloud | Upload a file from your phone, download it on your computer |
| Pi-hole | Visit any ad-heavy website — ads should be gone |
| Jellyfin | Stream a video on your phone |

---

## Stress Test

Now let's prove it works:

1. **Restart your server** (`sudo reboot`) — When it comes back, check that your service is running
2. **Turn off WiFi on your phone** and use mobile data — Can you still access your service on your home network? (No, and that's expected — see Chapter 7 for remote access)
3. **Delete the container** (`docker rm -f servicename`) and recreate it from your deployment command — Your data should still be there (because of volumes)

> **🔥 The Chaos Champion:** For Pi-hole specifically — unplug your router, wait 30 seconds, plug it back in. When your internet is back, try loading a website with ads. If Pi-hole is working, the ads won't load even though your internet is working. That's the magic of DNS-level blocking.

---

## 🔵 The Why

### Why Deploy a Service Instead of Just Running Docker?

Docker by itself is just a tool. A service is where Docker becomes **useful**. When you deploy Vaultwarden, Nextcloud, Pi-hole, or Jellyfin, you're not just running a container — you're:

- Replacing a subscription you pay for
- Taking control of your data
- Learning the patterns that scale to any service

The Docker command you run today is the same pattern you'll use for every service you deploy in the future. This chapter teaches you the **deploy, configure, verify** loop that's at the heart of homelabbing.

---

## 🟣 Deep Dive

### Multi-Service Considerations

When you're running multiple services, think about:

1. **Resource allocation:** Each service needs CPU and RAM. Monitor usage with `docker stats`.
2. **Network isolation:** Services shouldn't need to talk to each other unless they do. Use Docker networks to separate services.
3. **Update frequency:** Some services (Pi-hole) rarely need updating. Others (Vaultwarden) get security patches frequently. Stay on top of updates.

> **🚀 Turbo:** Once you're comfortable with single-service deployments, check out Chapter 8 where we combine everything into a single Docker Compose stack with clean URLs, SSL, and monitoring.

---

## 💼 Career Boost

**What you learned that employers care about:**
- Service deployment and configuration
- Data persistence with Docker volumes
- Network configuration (port mapping, DNS)
- Security basics (passwords, 2FA)
- Real-world problem solving (choosing the right tool for the job)

**Interview talking point:**
> *"I designed and deployed a self-hosted password management infrastructure using Vaultwarden (Bitwarden-compatible), serving my household of 4 people with end-to-end encrypted password storage. The system includes automated backups and runs on minimal hardware (₱5,000 Raspberry Pi)."*

(Adapt this to whichever service you chose.)

---

## 🇵🇭 PH Context

### Cost Savings Calculator

| Service | Philippine Cost (Monthly) | Self-Hosted Cost | Annual Savings |
|---|---|---|---|
| LastPass/1Password | ₱24-₱167 | ₱0 (free) | ₱288-₱2,004 |
| Google One 100GB | ₱59 | ₱0 (reuse storage) | ₱708 |
| Netflix Standard | ₱199 | ₱0 (Jellyfin) | ₱2,388 |
| Ad blocking (time saved) | Hard to measure | Pi-hole | Time = money |

**Total potential savings: ₱3,000-₱5,000+/year** for services you'll actually use.

### Mobile-First Design

Most Filipinos access the internet primarily from phones. All four services above have excellent mobile experiences:

- **Vaultwarden:** Official Bitwarden app (iOS + Android)
- **Nextcloud:** Official Nextcloud app (iOS + Android)
- **Pi-hole:** Web Dashboard (mobile browser) + Admin Panel
- **Jellyfin:** Jellyfin app (Android), browser-based (iOS)

### Storage Considerations

- **MicroSD cards** (for Raspberry Pi): Limited write cycles. For services that write frequently (Pi-hole logs, Nextcloud indexing), consider using an external USB SSD instead.
- **SSD vs HDD:** SSDs are quieter, faster, and more reliable for homelab use. A 256GB SSD costs ~₱1,500-₱2,500 on Shopee.

---

## What's Next

You've deployed your first real service. It's running, it's useful, and it's yours. In Chapter 5, we'll make sure it stays running — even when your server reboots, even when Docker crashes, even during a brownout.

**Homework:**
1. Configure your chosen service with your real data
2. Access it from at least 2 different devices
3. Set up 2FA if available
4. Take a screenshot of your service running — you'll want this for your portfolio (Chapter 13)

---

> **💸 Lean Path:** Every service in this chapter is completely free. Vaultwarden, Nextcloud, Pi-hole, and Jellyfin are all open source with no paid tiers. You're replacing subscriptions that cost ₱24-₱349/month with services that cost ₱0. The only investment is your time — and that time pays for itself in the first month.

---

## Go Deeper

- [Vaultwarden Documentation](https://docs.vaultwarden.org/) — Complete setup guide
- [Nextcloud Documentation](https://docs.nextcloud.com/) — Full admin guide
- [Pi-hole Documentation](https://docs.pi-hole.net/) — DNS blocking deep dive
- [Jellyfin Documentation](https://jellyfin.org/docs/) — Media server guide
- [Docker Volumes Explained](https://docs.docker.com/storage/volumes/) — Understanding persistent storage
