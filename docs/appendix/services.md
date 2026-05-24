# Service Directory

_Self-hosted services and tools for your homelab, with complexity ratings, resource requirements, and recommendation paths._

---

## Services Covered in This Book

> **Quick Reference:** Deployment — **[Chapter 4](../chapters/chapter-04.md)**. Reverse proxy — **[Chapter 7](../chapters/chapter-07.md)**. Monitoring — **[Chapter 11](../chapters/chapter-11.md)**.

These service notes are written for Filipino beginners first: they call out local cost pressure, mobile-first usage, and ISP realities where it matters.

---

### Uptime Kuma — Monitoring

| Field | Value |
|---|---|
| **Purpose** | Monitor service uptime and get alerts when things go down |
| **Docker Image** | `louislam/uptime-kuma:1` |
| **Default Port** | 3001 |
| **Complexity** | ⭐ (Beginner) |
| **CPU** | Minimal |
| **RAM** | ~100-200MB |
| **Storage** | ~50MB |
| **Use Case** | Beginners — easiest monitoring solution, beautiful dashboard |
| **Setup Time** | 15 minutes |
| **CHAP REF** | Chapter 11 |
| **PH Notes** | Telegram bot alerts work well in PH (most Filipinos use Telegram). Data usage is minimal (~5MB/month). |

---

### Vaultwarden — Password Manager

| Field | Value |
|---|---|
| **Purpose** | Self-hosted password manager (Bitwarden-compatible) |
| **Docker Image** | `vaultwarden/server:latest` |
| **Default Port** | 8080 |
| **Complexity** | ⭐⭐ (Easy) |
| **CPU** | Minimal |
| **RAM** | ~20-50MB |
| **Storage** | ~10MB database |
| **Use Case** | Replacing 1Password/LastPass — everyone needs this |
| **Setup Time** | 30 minutes |
| **CHAP REF** | Chapter 4 |
| **PH Notes** | Saves ₱24/month vs LastPass. Works great on any hardware. Family plan: share one instance with up to 5 family members. |

---

### Nextcloud — File Storage & Collaboration

| Field | Value |
|---|---|
| **Purpose** | Cloud storage, file sharing, calendar, contacts, documents |
| **Docker Image** | `nextcloud:latest` |
| **Default Port** | 8081 |
| **Complexity** | ⭐⭐⭐ (Intermediate) |
| **CPU** | Low-Medium |
| **RAM** | ~300-500MB |
| **Storage** | Depends on your files (start with 10GB+) |
| **Use Case** | Replacing Google Drive/Dropbox — family file sharing |
| **Setup Time** | 1 hour |
| **CHAP REF** | Chapter 4, Chapter 8 |
| **PH Notes** | Google One 100GB = ₱59/month. Nextcloud is free. Large files may be slow to upload depending on your PLDT/Globe upload speed. Consider a local-only setup if upload speed is below 10Mbps. |

---

### Pi-hole — Ad Blocking

| Field | Value |
|---|---|
| **Purpose** | DNS-level ad blocking for your entire network |
| **Docker Image** | `pihole/pihole:latest` |
| **Default Port** | 53 (DNS), 8082 (Admin) |
| **Complexity** | ⭐ (Beginner) |
| **CPU** | Minimal |
| **RAM** | ~50-100MB |
| **Storage** | ~100MB (blocklists grow ~10MB/year) |
| **Use Case** | Blocking ads on all devices simultaneously — phones, smart TVs, IoT |
| **Setup Time** | 30 minutes |
| **CHAP REF** | Chapter 4, Chapter 6 |
| **PH Notes** | Blocks ads on mobile data too (once set as router DNS). Works great on Raspberry Pi. Set it as your router's DNS to cover all devices automatically. |

---

### Jellyfin — Media Server

| Field | Value |
|---|---|
| **Purpose** | Stream your movies, TV shows, music, and photos |
| **Docker Image** | `jellyfin/jellyfin:latest` |
| **Default Port** | 8096 |
| **Complexity** | ⭐⭐ (Easy) |
| **CPU** | Medium (transcoding requires CPU power) |
| **RAM** | ~200-500MB |
| **Storage** | Depends on your media library |
| **Use Case** | Replacing Netflix for your own content — family movie nights |
| **Setup Time** | 45 minutes |
| **CHAP REF** | Chapter 4 |
| **PH Notes** | Netflix = ₱199/month. Jellyfin is free. 1080p streaming needs ~5Mbps upload. For 4K, you need 25Mbps+ upload (most PH plans don't support this). Convert 4K to 1080p first for best results. Intel Quick Sync (NUC, 8th-gen+ i5) helps with transcoding. |

---

### Caddy — Reverse Proxy

| Field | Value |
|---|---|
| **Purpose** | Route traffic to your services using clean URLs with auto-SSL |
| **Docker Image** | `caddy:latest` |
| **Default Port** | 80, 443 |
| **Complexity** | ⭐⭐ (Easy) |
| **CPU** | Minimal |
| **RAM** | ~10-30MB |
| **Storage** | ~10MB |
| **Use Case** | Clean URLs, automatic HTTPS, the glue that holds your homelab together |
| **Setup Time** | 30 minutes |
| **CHAP REF** | Chapter 7 |
| **PH Notes** | Works through CGNAT for local access. For remote public access, pair with Cloudflare Tunnel. Cheap domains (~₱560/year) work great. |

---

### Prometheus — Metrics Collection

| Field | Value |
|---|---|
| **Purpose** | Collect and store time-series metrics from your services |
| **Docker Image** | `prom/prometheus:latest` |
| **Default Port** | 9090 |
| **Complexity** | ⭐⭐⭐ (Intermediate) |
| **CPU** | Low |
| **RAM** | 200-500MB (grows with retention period) |
| **Storage** | ~1GB+ (depends on metrics volume and retention) |
| **Use Case** | Advanced monitoring and alerting — the backbone of visibility |
| **Setup Time** | 1 hour |
| **CHAP REF** | Chapter 11 |
| **PH Notes** | Minimal data usage (metrics are small). Set retention to 15 days on low-storage setups. |

---

### Grafana — Visualization

| Field | Value |
|---|---|
| **Purpose** | Create dashboards from Prometheus metrics |
| **Docker Image** | `grafana/grafana-oss:latest` |
| **Default Port** | 3000 |
| **Complexity** | ⭐⭐⭐ (Intermediate) |
| **CPU** | Minimal |
| **RAM** | 100-200MB |
| **Storage** | ~50MB |
| **Use Case** | Beautiful, interactive dashboards for your homelab metrics |
| **Setup Time** | 30 minutes |
| **CHAP REF** | Chapter 11 |
| **PH Notes** | Works great on any hardware. Use pre-built dashboard templates from the Grafana dashboard library. |

---

### Node Exporter — Server Metrics

| Field | Value |
|---|---|
| **Purpose** | Expose server hardware metrics (CPU, RAM, disk, network) |
| **Docker Image** | `prom/node-exporter:latest` |
| **Default Port** | 9100 |
| **Complexity** | ⭐ (Beginner) |
| **CPU** | Negligible |
| **RAM** | ~10MB |
| **Storage** | None |
| **Use Case** | Server-level monitoring — feed into Prometheus + Grafana |
| **Setup Time** | 10 minutes |
| **CHAP REF** | Chapter 11 |
| **PH Notes** | Extremely lightweight. Run on every server you own. |

---

### cAdvisor — Container Metrics

| Field | Value |
|---|---|
| **Purpose** | Expose per-container resource usage metrics |
| **Docker Image** | `gcr.io/cadvisor/cadvisor:latest` |
| **Default Port** | 8080 |
| **Complexity** | ⭐ (Beginner) |
| **CPU** | Negligible |
| **RAM** | ~50-100MB |
| **Storage** | None |
| **Use Case** | Container-level monitoring — see which services use the most resources |
| **Setup Time** | 10 minutes |
| **CHAP REF** | Chapter 11 |
| **PH Notes** | Minimal overhead. Useful for identifying resource hogs on low-RAM setups. |

---

### Watchtower — Auto Updates

| Field | Value |
|---|---|
| **Purpose** | Automatically update Docker containers when new images are available |
| **Docker Image** | `containrrr/watchtower:latest` |
| **Default Port** | None (runs in background) |
| **Complexity** | ⭐ (Beginner) |
| **CPU** | Negligible |
| **RAM** | ~10-20MB |
| **Storage** | None |
| **Use Case** | Keeping all containers up to date without manual intervention |
| **Setup Time** | 10 minutes |
| **CHAP REF** | Chapter 10 |
| **PH Notes** | Container pulls happen over your internet connection. Schedule during off-peak hours if on a data-capped plan. |

---

### WireGuard — VPN

| Field | Value |
|---|---|
| **Purpose** | Secure VPN for remote access to your homelab |
| **Docker Image** | `wgh1/wireguard` or `linuxserver/wireguard` |
| **Default Port** | 51820 (UDP) |
| **Complexity** | ⭐⭐ (Easy) |
| **CPU** | Minimal |
| **RAM** | ~10-20MB |
| **Storage** | None |
| **Use Case** | Secure remote access, alternative to Tailscale |
| **Setup Time** | 30 minutes |
| **CHAP REF** | Chapter 7, Chapter 12 |
| **PH Notes** | WireGuard is extremely fast and efficient — great for PH's variable internet conditions. Works through CGNAT since it initiates outgoing connections. |

---

### Tailscale — Mesh VPN

| Field | Value |
|---|---|
| **Purpose** | Zero-config mesh VPN for secure remote access |
| **Docker Image** | `tailscale/tailscale:latest` |
| **Default Port** | 41641 (UDP) |
| **Complexity** | ⭐ (Beginner) |
| **CPU** | Minimal |
| **RAM** | ~20-40MB |
| **Storage** | ~10MB |
| **Use Case** | Easiest remote access setup — 5 minutes, works through CGNAT |
| **Setup Time** | 5 minutes |
| **CHAP REF** | Chapter 7 |
| **PH Notes** | Free for personal use (up to 100 devices). Works perfectly through PLDT/Globe/DITO CGNAT. MagicDNS makes service discovery trivial. |

---

### Cloudflared — Cloudflare Tunnel

| Field | Value |
|---|---|
| **Purpose** | Expose services to the internet securely without port forwarding |
| **Docker Image** | `cloudflare/cloudflared:latest` |
| **Default Port** | None (outgoing connection) |
| **Complexity** | ⭐⭐⭐ (Intermediate) |
| **CPU** | Minimal |
| **RAM** | ~20-40MB |
| **Storage** | ~10MB |
| **Use Case** | Public-facing services without opening router ports |
| **Setup Time** | 20 minutes |
| **CHAP REF** | Chapter 7 |
| **PH Notes** | Free tier supports unlimited tunnels. Works through any CGNAT. Requires a domain name (~₱560/year via Cloudflare Registrar). |

---

## Bonus: Services to Explore Next

_Services not covered in the book but great additions to a homelab._

| Service | Purpose | Docker Image | Port | Complexity | CPU | RAM | Setup Time | Why Add It |
|---|---|---|---|---|---|---|---|---|
| **Gitea** | Self-hosted Git | `gitea/gitea:latest` | 3000 | ⭐⭐ | Low | ~100MB | 20 min | Your own GitHub — great for learning |
| **Homepage** | Dashboard | `gethomepage/homepage:latest` | 3000 | ⭐ | Minimal | ~10MB | 10 min | Beautiful homelab dashboard |
| **FileBrowser** | Web file manager | `filebrowser/filebrowser:latest` | 80 | ⭐ | Minimal | ~10MB | 5 min | Web-based file management |
| **Vault** | Secrets management | `hashicorp/vault:latest` | 8200 | ⭐⭐⭐⭐ | Low | ~100MB | 45 min | Enterprise-grade secrets |
| **MinIO** | S3-compatible storage | `minio/minio:latest` | 9000 | ⭐⭐ | Low | ~100MB | 20 min | Your own cloud storage backend |
| **Memos** | Note-taking | `neko8657/memos:latest` | 5230 | ⭐ | Minimal | ~30MB | 10 min | Simple, private notes |
| **Outline** | Knowledge base | `getoutline/outline:server` | 3000 | ⭐⭐⭐ | Medium | ~300MB | 45 min | Your own Wikipedia |
| **Paperless-NG** | Document management | `paperless/paperless:latest` | 8000 | ⭐⭐⭐ | Medium | ~500MB | 1 hour | Scan and organize documents |
| **Home Assistant** | Smart home | `homeassistant/home-assistant:latest` | 8123 | ⭐⭐ | Low | ~300MB | 30 min | Automate your home |
| **Plausible** | Analytics | `plausible/analytics:latest` | 8000 | ⭐⭐⭐ | Medium | ~400MB | 45 min | Privacy-friendly Google Analytics |
| **Radarr/Sonarr** | Media automation | `linuxserver/radarr:latest` | 7878 | ⭐⭐⭐ | Low-Med | ~200MB | 30 min | Auto-download movies/shows |
| **Immich** | Photo management | `ghcr.io/immich-app/immich-server` | 2283 | ⭐⭐⭐ | Medium | ~500MB | 45 min | Google Photos alternative |
| **FreshRSS** | RSS reader | `linuxserver/freshrss:latest` | 8088 | ⭐ | Minimal | ~30MB | 15 min | Read all your feeds in one place |
| **Baritone** | CalDAV/CardDAV | `baridon/baritone:latest` | 8383 | ⭐⭐ | Minimal | ~20MB | 15 min | Calendar and contacts sync |
| **Stirling-PDF** | PDF tools | `frooodle/s-pdf:latest` | 8080 | ⭐ | Minimal | ~50MB | 10 min | Merge, split, convert PDFs |

---

## Recommendation Paths

_Chose your first services based on who you are and what you need._

### 🟢 "First 5 Services" — For Absolute Beginners

Start here. These 5 services cover the basics and teach you the core patterns:

| Order | Service | Why First? | Time |
|---|---|---|---|
| 1 | **Uptime Kuma** | See what's running — instant gratification | 15 min |
| 2 | **Vaultwarden** | Replaces a paid subscription — immediate value | 30 min |
| 3 | **Pi-hole** | Block ads everywhere — visible impact | 30 min |
| 4 | **Caddy** | Clean URLs — makes everything feel professional | 30 min |
| 5 | **Watchtower** | Set and forget — automation win | 10 min |

**Total setup time:** ~2 hours  
**Total RAM usage:** ~400MB  
**Total monthly cost:** ₱0

---

### 🏠 "Family-Focused" Stack

Services that benefit your whole household:

| Service | Purpose | Family Benefit |
|---|---|---|
| **Nextcloud** | Shared files, family photos, calendar | Everyone has access to shared files |
| **Jellyfin** | Family movie/TV streaming | No more Netflix subscription |
| **Vaultwarden** | Shared passwords (with family plan) | One password manager for everyone |
| **Pi-hole** | Ads blocked for all devices | Cleaner experience for kids' devices |
| **Uptime Kuma** | Monitor all family services | Know when something's down |

**Total monthly savings:** ~₱250-₱500 (Netflix + Google One + LastPass)

---

### 💻 "Developer-Focused" Stack

Services that make you more productive as a developer:

| Service | Purpose | Dev Benefit |
|---|---|---|
| **Gitea** | Self-hosted Git | Your own code repository |
| **Vaultwarden** | Password manager | Secure credentials management |
| **Uptime Kuma** | Monitor your services | Know when deployments break |
| **Paperless-NG** | Document management | Invoice/receipt organization |
| **Caddy** | Reverse proxy | Clean URLs for dev environments |
| **Memos** | Private notes | Developer journal, snippets |

---

### 🔒 "Privacy-First" Stack

Services that maximize your data ownership:

| Service | Purpose | Privacy Benefit |
|---|---|---|
| **Vaultwarden** | Password manager | No cloud password storage |
| **Nextcloud** | File storage | Your files stay on your server |
| **Pi-hole** | DNS-level ad/blocking | No tracking at the network level |
| **Plausible** | Website analytics | No cookies, no tracking pixels |
| **Tailscale** | Encrypted remote access | End-to-end encrypted tunnels |
| **Vault** | Secrets management | Enterprise-grade encryption |

---

### 💸 "₱0 Budget" Stack

All services that run on your existing laptop or a Raspberry Pi:

| Service | RAM | Why It Works on Minimal Hardware |
|---|---|---|
| **Uptime Kuma** | ~100MB | Lightweight, single binary |
| **Vaultwarden** | ~20MB | Extremely efficient Rust implementation |
| **Pi-hole** | ~50MB | DNS only, almost no compute needed |
| **Caddy** | ~10MB | Written in Go, tiny memory footprint |
| **Watchtower** | ~15MB | Runs occasionally, idle most of the time |

**Total RAM:** ~200MB — fits on a Raspberry Pi 4 with 4GB easily.

---

### 🧪 "Advanced / Future" Stack

These are not core MVP services, but they match the later `Postdoc` paths of the book:

| Service | Purpose | Why It Belongs Later |
|---|---|---|
| **Home Assistant** | Home automation hub | Moves the homelab closer to household control |
| **Ollama** | Local LLM runtime | Useful private AI, but hardware needs vary a lot |
| **Game server** | Host a game for family or friends | Great homelab use case, but direct access and recovery matter |
| **Dokploy** | Container deployment manager | Good once the stack is big enough to need an operations desk |

**Rule of thumb:** if a service needs more structure than a single `docker run`, it probably belongs in the later part of the book or a deep-dive appendix.

---

## Resource Capacity Planner

_Estimate what your homelab needs based on the services you want._

| Services Count | Min RAM | Recommended RAM | Suitable Hardware |
|---|---|---|---|
| 1-3 (Lite) | 2GB | 4GB | Raspberry Pi 4, old laptop |
| 4-6 (Standard) | 6GB | 8GB | Mini PC (N100, i5 8th gen) |
| 7-10 (Heavy) | 12GB | 16GB | Tower PC (i5/i7 8th-9th gen) |
| 10+ (Power User) | 24GB | 32GB+ | Multi-server or beefy single server |

> **💸 Lean Path:** You don't need all of these. Start with ONE service that solves a real problem for you. Add more when you need them. A homelab with one useful service is better than a homelab with ten unused ones.
