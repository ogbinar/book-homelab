# Chapter 8: Multiple Services, One System

## What You'll Build
A well-organized Docker Compose stack that manages all your homelab services from a single file. Shared networks, shared volumes, environment variables, and a clean directory structure that makes adding and removing services effortless.

## How Long It Takes
3 hours (including reading, refactoring, and testing)

## What You Need
- Your server with Docker and multiple services running
- The reverse proxy from Chapter 7 (optional but recommended)
- All services you've deployed so far

---

## The Story

Right now, your services are probably scattered across different directories, each started with its own `docker run` command or separate `docker-compose.yml` file. It works, but it's messy.

Adding a new service means:
1. Creating a new directory
2. Running a new docker command
3. Updating the reverse proxy
4. Updating your backup script
5. Remembering all of this next time you rebuild

What if you could manage your entire homelab from one file? One command to start everything. One command to back up everything. One command to rebuild from scratch? *Imagine kung sa-isang command, lahat ng services mo ay nabubuhay ulit. No more hunting for config files sa iba't-ibang directories.*

That's what a **Docker Compose stack** is — a single file that defines every service, every network, every volume, and every environment variable for your entire homelab.

---

## Why This Matters

Infrastructure as Code (IaC) starts with Docker Compose. If you can define your entire homelab in a text file, you can:

1. **Rebuild from scratch** in minutes, not hours
2. **Share your setup** with others (or yourself on another machine)
3. **Version control** your configuration with Git
4. **Understand your architecture** at a glance
5. **Scale systematically** as you add more services

> **📢 Jargon Alert:** "Infrastructure as Code" — Managing and provisioning computing infrastructure through machine-readable definition files, instead of manual configuration. Docker Compose is your first step into IaC.

---

## 🟢 Quick Start

### Step 1: Create the Stack Directory Structure

```bash
# Create the main directory
mkdir -p ~/homelab && cd ~/homelab

# Create subdirectories for each service
mkdir -p services/{uptime-kuma,vaultwarden,nextcloud,jellyfin,pihole,caddy}
mkdir -p data/{kuma,vault,nextcloud,jellyfin,pihole}
mkdir -p config/{caddy,pihole}
```

Your structure should look like:
```
~/homelab/
├── docker-compose.yml
├── services/
│   ├── uptime-kuma/
│   ├── vaultwarden/
│   ├── nextcloud/
│   ├── jellyfin/
│   ├── pihole/
│   └── caddy/
├── data/
│   ├── kuma/
│   ├── vault/
│   ├── nextcloud/
│   ├── jellyfin/
│   └── pihole/
└── config/
    ├── caddy/
    └── pihole/
```

### Step 2: Create the Master docker-compose.yml

```bash
nano ~/homelab/docker-compose.yml
```

```yaml
# docker-compose.yml — Homelab Master Stack
# Version: 1.0
# Last Updated: 2026-05-16

version: "3.8"

services:
  # ─── Monitoring ──────────────────────────────────────
  uptime-kuma:
    image: louislam/uptime-kuma:1
    container_name: uptime-kuma
    restart: unless-stopped
    ports:
      - "3001:3001"
    volumes:
      - ./data/kuma:/app/data
    networks:
      - homelab

  # ─── Security ────────────────────────────────────────
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    ports:
      - "8080:80"
    volumes:
      - ./data/vault:/data
    environment:
      - ADMIN_TOKEN=${ADMIN_TOKEN:-your-admin-token-here}
      - SIGNUPS_ALLOWED=false
    networks:
      - homelab

  # ─── Storage ─────────────────────────────────────────
  nextcloud:
    image: nextcloud:latest
    container_name: nextcloud
    restart: unless-stopped
    ports:
      - "8081:80"
    volumes:
      - ./data/nextcloud:/var/www/html/data
      - ./data/nextcloud/config:/var/www/html/config
    environment:
      - MYSQL_HOST=db
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=${NC_DB_PASSWORD:-changeme}
    depends_on:
      - db
    networks:
      - homelab
      - db

  # Database for Nextcloud (internal only)
  db:
    image: mariadb:10.6
    container_name: nextcloud-db
    restart: unless-stopped
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    volumes:
      - ./data/nextcloud/db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=${DB_ROOT_PASSWORD:-changeme}
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=${NC_DB_PASSWORD:-changeme}
    networks:
      - db

  # ─── Media ───────────────────────────────────────────
  jellyfin:
    image: jellyfin/jellyfin:latest
    container_name: jellyfin
    restart: unless-stopped
    ports:
      - "8096:8096"
    volumes:
      - ./data/jellyfin/config:/config
      - ./data/jellyfin/cache:/cache
      - /media:/media:ro
    networks:
      - homelab

  # ─── Network ─────────────────────────────────────────
  pihole:
    image: pihole/pihole:latest
    container_name: pihole
    restart: unless-stopped
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "8082:80"
    volumes:
      - ./config/pihole:/etc/pihole
      - ./config/pihole/dnsmasq:/etc/dnsmasq.d
    environment:
      - TZ=Asia/Manila
      - WEBPASSWORD=${PIHOLE_PASSWORD:-changeme}
    networks:
      - homelab

  # ─── Reverse Proxy ───────────────────────────────────
  caddy:
    image: caddy:latest
    container_name: caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
    volumes:
      - ./config/caddy/Caddyfile:/etc/caddy/Caddyfile
      - ./data/caddy-data:/data
      - ./data/caddy-config:/config
    network_mode: host
    depends_on:
      - uptime-kuma
      - vaultwarden
      - nextcloud
      - jellyfin
      - pihole

# ─── Networks ──────────────────────────────────────────
networks:
  homelab:
    driver: bridge
  db:
    driver: bridge
    internal: true  # No external access for database

# ─── Volumes ───────────────────────────────────────────
volumes:
  kuma-data:
  vault-data:
  nextcloud-data:
  nextcloud-config:
  nextcloud-db:
  jellyfin-config:
  jellyfin-cache:
```

### Step 3: Customize the Stack

This compose file includes every service you've deployed so far. If you only deployed some of them (e.g., you only chose Vaultwarden in Chapter 4), remove the services you don't have. The compose file is a template — customize it to your setup.

For example, if you don't have Pi-hole or Jellyfin, delete their entire service blocks and their volumes from the `volumes:` section at the bottom. Keep the services and volumes that match what you've deployed.

### Step 4: Create an Environment File

```bash
# Create .env file for secrets
nano ~/homelab/.env
```

```bash
# Admin token for Vaultwarden (generate with: openssl rand -base64 48)
ADMIN_TOKEN=your-generated-admin-token

# Database passwords
NC_DB_PASSWORD=strong-random-password-here
DB_ROOT_PASSWORD=another-strong-password-here

# Pi-hole admin password
PIHOLE_PASSWORD=your-pihole-password
```

> **⚠️ Watch Out:** Never commit your `.env` file to Git. Add it to `.gitignore`:
> ```bash
> echo ".env" >> ~/homelab/.gitignore
> ```

### Step 5: Stop Old Containers and Start the Stack

```bash
# Stop all old containers (one by one)
docker stop uptime-kuma
docker stop vaultwarden
docker stop nextcloud
docker stop jellyfin
docker stop pihole

# Remove old containers (optional, if you want a clean start)
docker rm uptime-kuma
docker rm vaultwarden
docker rm nextcloud
docker rm jellyfin
docker rm pihole

# Start everything from the stack
cd ~/homelab
docker compose up -d
```

### Step 6: Verify Everything Works

```bash
# Check all containers are running
docker compose ps

# View all logs
docker compose logs

# View logs for a specific service
docker compose logs -f caddy
```

**Expected output:**
```
NAME              IMAGE                    COMMAND             SERVICE        STATUS
caddy             caddy:latest             "caddy run"         caddy          running
jellyfin          jellyfin/jellyfin:latest "jellyfin"          jellyfin       running
nextcloud         nextcloud:latest         "apache2-foregro"   nextcloud      running
pihole            pihole/pihole:latest     "start.sh"          pihole         running
uptime-kuma       louislam/uptime-kuma:1   "./node server"     uptime-kuma    running
vaultwarden       vaultwarden/server:latest"start.sh"            vaultwarden    running
nextcloud-db      mariadb:10.6             "mariadbd"          db             running
```

---

## 🔵 The Why

### Docker Compose File Structure

A Docker Compose file has four main sections:

1. **services** — The containers to run (what we've been doing)
2. **networks** — How containers talk to each other
3. **volumes** — Persistent storage definitions
4. **environment** (in `.env`) — Configuration variables

### Network Isolation

Notice the two networks in our compose file:

```yaml
networks:
  homelab:   # Public — containers can be reached from outside
  db:         # Internal — only nextcloud and db can talk here
```

The `db` network is marked `internal: true`, meaning:
- The database container can talk to Nextcloud
- But the database CANNOT be reached from outside the stack
- This is a security best practice

```
Internet ──▶ Caddy (network_mode: host)
                │
                ▼
          ┌──────────┐
          │ homelab  │◀── All services connected here
          │  Network │
          └────┬─────┘
               │
        ┌──────┼──────┐
        ▼      ▼      ▼
     Kuma  Vault  Jellyfin
              │
         ┌────▼────┐
         │  db     │◀── Nextcloud connects here
         │ Network │    (internal only!)
         └─────────┘
```

### Environment Variables

The `.env` file keeps secrets out of your compose file. This is critical for:
- Database passwords
- API keys
- Admin tokens
- Any sensitive configuration

Docker Compose automatically loads `.env` files in the same directory. You reference them with `${VARIABLE_NAME:-default_value}` syntax.

> **📢 Jargon Alert:** `${VARIABLE:-default}` — If `VARIABLE` is set, use its value. If not, use `default`. The `:-` means "use this default if the variable is empty or unset."

---

## 🟣 Deep Dive

### Organizing Large Stacks

As your homelab grows, a single `docker-compose.yml` becomes unwieldy. Use **compose profiles** or **include** to split it up:

```yaml
# docker-compose.yml — Main file
version: "3.8"

includes:
  - ./services/monitoring
  - ./services/security
  - ./services/storage
  - ./services/media
  - ./services/network
  - ./services/proxy
```

Each file in `services/` contains its own services section:

```yaml
# services/monitoring/compose.yml
version: "3.8"
services:
  uptime-kuma:
    image: louislam/uptime-kuma:1
    ...
```

### Docker Compose Commands Reference

```bash
# Start all services
docker compose up -d

# Stop all services
docker compose down

# Restart all services
docker compose restart

# Restart a single service
docker compose restart vaultwarden

# View logs (all services)
docker compose logs -f

# View logs (specific service)
docker compose logs -f caddy

# Update all images
docker compose pull
docker compose up -d

# Recreate all containers (apply config changes)
docker compose up -d --force-recreate

# Check for config errors
docker compose config

# Execute a command inside a running container
docker compose exec caddy caddy version
```

---

## 💼 Career Boost

**What you learned that employers care about:**
- Docker Compose multi-service orchestration
- Network segmentation in container environments
- Secrets management with environment variables
- Infrastructure definition as code
- Service dependency management

**Interview talking point:**
> *"I designed a multi-service Docker Compose stack managing 7+ containers with network segmentation, volume-based persistence, and environment-variable-based configuration. The stack includes service dependencies, internal-only database networks, and automated health checks — demonstrating production-grade container orchestration patterns."*

---

## 🇵🇭 PH Context

### Storage Planning

As you add more services, your data directory grows:

| Service | Monthly Data Growth |
|---|---|
| Uptime Kuma | ~100MB (logs) |
| Vaultwarden | ~1MB (password database) |
| Nextcloud | Varies (your files) |
| Jellyfin | Varies (media files) |
| Pi-hole | ~50MB (query logs) |

**Recommendation:** Start with a 256GB SSD (₱1,500-₱2,500). Upgrade to 1TB+ when you hit 70% capacity.

> **💸 Lean Path:** GitHub public repositories are free. Private repositories are also free (unlimited repos, up to 2 collaborators on free plan). Your homelab config doesn't need to be secret — it's just Docker configs. Use a public repo for your portfolio and a private one if you store `.env` with real passwords.

### Git for Configuration

Store your `docker-compose.yml` and `.env` in a private Git repository:

```bash
cd ~/homelab
git init
git add .
git commit -m "Initial homelab stack configuration"
```

This gives you:
- Version history (what changed and when) — *kasi next month, hindi mo na naalala kung ano ang ginawa mo kanina*
- Easy migration to new hardware
- A portfolio piece for your CV (Chapter 13)

> **⚠️ Security reminder:** NEVER commit your `.env` file. Use `.gitignore`.

---

## Stress Test

Now let's prove your Docker Compose stack is resilient:

1. **Run a full stack restart** — `docker compose down && docker compose up -d`. Every service should come back up in the right order. Check each one in your browser.
2. **Delete a single service's data directory** (e.g., `rm -rf data/kuma/*`). Then recreate just that container: `docker compose up -d uptime-kuma`. Its data should be gone — which is why backups matter (Chapter 9).
3. **Remove one service from docker-compose.yml** (e.g., delete the nextcloud section). Run `docker compose config` to verify the file is valid YAML. Then add it back. This proves you understand the compose file structure.
4. **Change a network name** in docker-compose.yml. Run `docker compose up -d`. The new network is created, but old containers are on the old network. They won't communicate. This teaches you about Docker network persistence.

> **🔥 The Chaos Champion:** Delete your entire `~/homelab` directory. Then recreate it from memory using only your Git history. If you've been committing (you should be), you can restore everything: `git clone your-repo ~/homelab && cd ~/homelab && docker compose up -d`. This is the ultimate proof that Infrastructure as Code works — your entire homelab lives in a text file.

---

## What's Next

Your homelab is now a single, organized system. In Chapter 9, we'll make sure all that data is backed up properly — with automated, tested, offsite backup strategies.

**Homework:**
1. Migrate all your services to the unified docker-compose stack
2. Test that every service works through the stack
3. Set up Git version control for your configuration
4. Practice a full stack restart: `docker compose down && docker compose up -d`

---

> **🚀 Turbo:** Want to manage multiple environments? Use Docker Compose profiles: add `profiles: ["development"]` to a service, then run `docker compose --profile development up -d` to selectively start services. You can also override your compose file with environment-specific configs: create `docker-compose.override.yml` for local tweaks that aren't committed to Git. Run `docker compose -f docker-compose.yml -f docker-compose.override.yml up -d` to merge both files.

---

## Go Deeper

- [Docker Compose File Reference](https://docs.docker.com/compose/compose-file/) — Complete spec
- [Docker Compose Best Practices](https://docs.docker.com/compose/best-practices/) — Official best practices
- [Git Documentation](https://git-scm.com/doc) — Version control reference
