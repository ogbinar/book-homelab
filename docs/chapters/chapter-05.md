# Chapter 5: Keeping It Alive

## What You'll Build
A resilient homelab that survives reboots, crashes, and power outages. You'll set up auto-restart policies, health monitoring, and Docker cleanup — so your services stay running with minimal effort.

## How Long It Takes
45 minutes (including reading and hands-on steps)

## What You Need
- Your server from Chapter 2 with Docker and at least one service running (Chapter 4)
- Internet connection

---

## The Story

You deployed your first service. It works. You're proud. You tell your friends.

Two weeks later, your power goes out during a storm. When it comes back, your server reboots — but your service doesn't. You SSH in and find Docker is running, but your container isn't. You check the logs and see an error about a corrupted database file.

You spend 3 hours fixing it. Your service is back. But you make a promise to yourself: *Hindi na ulit 'to.*

That promise is what this chapter is about. The difference between a toy and a tool is **reliability**. Anyone can deploy something that works today. A homelabber builds systems that work tomorrow, next week, and next month — even when things go wrong.

Because they will.

---

## Why This Matters

In professional infrastructure, there's a concept called **Mean Time Between Failures (MTBF)**. It's how long a system runs before something breaks. In homelabbing, your MTBF is measured in days, not years. Power outages. Docker updates that break things. Disk errors. Network issues.

The goal isn't to prevent failures (impossible). The goal is to **recover quickly and automatically**.

> **📢 Jargon Alert:** "Idempotent" — Running this command 10 times has the same result as running it once. Like pressing the "play" button on Spotify — pressing it once or ten times, you get the same song playing. We want our infrastructure to be idempotent: no matter how many times we restart, the result is the same working system.

---

## 🟢 Quick Start

### Step 1: Verify Auto-Restart Policies

You already set up `--restart unless-stopped` in Chapter 3. Let's verify it works:

```bash
# Check if your container has the right restart policy
docker inspect --format='{{.HostConfig.RestartPolicy.Name}}' uptime-kuma
```

**Expected output:** `unless-stopped`

This means: Docker will automatically restart this container if it crashes or if the Docker daemon restarts — unless you explicitly stopped it.

**Other restart policies:**
- `no` — Never restart (default)
- `always` — Always restart, even if you manually stopped it first
- `on-failure` — Only restart if the container exits with an error
- `unless-stopped` — Restart always, except if you manually stopped it (recommended)

**Set it on any container:**
```bash
docker update --restart unless-stopped <container-name>
```

> **⚠️ The Pitfall:** If your service has a bug that causes it to crash in a loop, `always` or `unless-stopped` will keep restarting it forever. Use `on-failure` with a max retry count for buggy services:
> ```bash
> docker update --restart on-failure:5 <container-name>
> ```

### Step 2: Set Up Health Monitoring

Uptime Kuma (which you installed in Chapter 3) is already monitoring your services — but let's make sure it's actually watching the right things.

1. Open Uptime Kuma at `http://YOUR_SERVER_IP:3001`
2. Click "Add New Monitor"
3. Add monitors for:
   - Your homelab services (HTTP monitors on their ports)
   - Your server itself (Ping monitor on your server's IP)
   - Your internet connection (Ping monitor on 8.8.8.8)

**Example monitor settings:**
- **Type:** HTTP(s)
- **URL:** `http://localhost:3001` (for Uptime Kuma itself)
- **Interval:** 1 minute
- **Keywords:** `200` (alert if "200" doesn't appear in the response)

### Step 3: Keep Docker Clean

Over time, Docker accumulates unused images, stopped containers, and build cache. Clean regularly:

```bash
# Remove stopped containers
docker container prune

# Remove unused images
docker image prune -a

# Remove unused volumes
docker volume prune

# Remove everything unused (careful!)
docker system prune -a --volumes
```

**Schedule this monthly:**
```bash
# Add to crontab (first Sunday of each month at 4 AM)
0 4 1 * * docker system prune -f --volumes
```

> **⚠️ Watch Out:** `docker system prune -a --volumes` removes EVERYTHING unused, including volumes. Only use this if you're sure you have backups. For regular cleanup, use `docker system prune -f` (without `-a` and `--volumes`).

> **💡 Quick Win:** Run `docker system df` anytime to see how much disk space Docker is using. You'll be surprised.

---

## 🔵 The Why

### Why Auto-Restart Matters

Without auto-restart, every container crash means:
1. You notice something's down (maybe)
2. You SSH into your server
3. You check the logs
4. You start the container manually
5. You hope you did it right

With auto-restart, steps 1-5 happen automatically. You only get involved when something truly needs your attention.

### Why Monitoring Matters

Uptime Kuma doesn't just tell you something is down — it tells you **when** it went down and **how long** it was down. That history is gold when debugging:

> "Your service went down at 3:14 AM on Tuesday. It was down for 12 minutes. That lines up with a Docker update that ran at 3:00 AM."

### Why Docker Cleanup Matters

Docker is a bit of a hoarder. Every time you pull a new image, the old one stays on disk. Every time you stop a container, its data might persist. Over weeks, this adds up:

```bash
# Check Docker disk usage
docker system df
```

**Expected output:**
```
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          15        8         2.1GB     1.3GB (62%)
Containers      8         5         150MB     45MB (30%)
Local Volumes   10        8         3.2GB     800MB (25%)
Build Cache     0         0         0B        0B
```

Running `docker system prune` reclaimed 1.3GB. That's free space you didn't know you had.

---

## 🟣 Deep Dive

### Docker Volume Naming

When you use `-v ~/some/path:/container/path`, Docker creates an **anonymous volume** tracked internally. But when you use named volumes in Docker Compose, they're easier to manage:

```yaml
# docker-compose.yml example
version: "3.8"

volumes:
  kuma-data:
  vault-data:

services:
  uptime-kuma:
    volumes:
      - kuma-data:/app/data
  vaultwarden:
    volumes:
      - vault-data:/data
```

### Backup Awareness

A quick manual backup is all you need right now. Before you move on, copy your service data somewhere safe:

```bash
# Copy your service data to a backup folder
mkdir -p ~/homelab-backup
cp -r ~/uptime-kuma/data ~/homelab-backup/
cp -r ~/vaultwarden/data ~/homelab-backup/ 2>/dev/null || true
```

This is a basic safety net. In **Chapter 9**, you'll set up the full automated backup strategy with the 3-2-1 rule, external drives, and restore drills. For now, this manual copy is enough to prevent data loss while you keep learning.

### Backup Strategies Compared

| Strategy | Pros | Cons | Best For |
|---|---|---|---|
| **Manual copy** (above) | Simple, no setup | Easy to forget | Right now |
| **rsync to external drive** | Simple, fast, incremental | Manual setup | Beginners |
| **Docker volume backup** | Container-aware, automated | Tar files aren't searchable | Docker-heavy setups |
| **Restic** | Encrypted, deduplicated, cloud support | Steeper learning curve | Advanced users |
| **BorgBackup** | Excellent deduplication, encrypted | Linux-only, complex | Large datasets |

> **💸 The Lean Path:** If you don't have an external drive yet, your backup is "manual: copy important data to a friend's house or Google Drive." That's better than nothing.

---

## 💼 Career Boost

**What you learned that employers care about:**
- Container restart policies and reliability engineering
- Health monitoring and observability
- System maintenance and resource management
- Testing and validation procedures

**Interview talking point:**
> *"I implemented auto-restart policies and health monitoring for my homelab infrastructure using Docker's restart policies and Uptime Kuma, achieving 99%+ uptime with automated recovery from crashes and power outages."*

---

## 🇵🇭 PH Context

### Power Outage Reality

Philippine power stability varies by location:
- **Metro Manila:** Occasional brownouts (PLDT/MLP maintenance, accidents)
- **Provincial:** More frequent, longer outages
- **Typhoon season (June-Nov):** Extended outages possible (1-7 days)

**Your defense:**
1. **Laptop path:** The built-in battery handles short outages automatically
2. **Dedicated server:** Add a UPS (₱2,000-₱5,000). A basic 600VA UPS gives 15-30 minutes — enough for graceful shutdown
3. **Auto-shutdown script:**
```bash
# /etc/upsdrv.conf — For Basic UPS auto-shutdown
# Install apcupsd for basic UPS support
sudo apt install apcupsd

# Configure for auto-shutdown on low battery
sudo nano /etc/apcupsd/apcupsd.conf
# Set: BATTERYLEVEL 5
# Set: MINUTES 10
# Set: ONBATTERYDELAY 2
# Set: KILLPOWER (shuts down Docker and system on critical battery)
```

### Internet Outages

When the internet goes down:
- **Local services** (Pi-hole, Nextcloud on LAN, Jellyfin) — Still work on your home network
- **Remote access** — Won't work until internet is back
- **Docker updates** — Can't pull new images, but existing containers keep running

**Mitigation:** Keep important images cached locally:
```bash
docker pull louislam/uptime-kuma:1
docker pull vaultwarden/server:latest
docker pull nextcloud:latest
```

### Storage Costs

| Storage Type | Capacity | Price (₱) | Source |
|---|---|---|---|
| 128GB USB 3.0 drive | 128GB | ₱400-₱600 | Shopee/Lazada |
| 500GB portable HDD | 500GB | ₱1,500-₱2,000 | Shopee/Lazada |
| 1TB portable HDD | 1TB | ₱2,500-₱3,500 | Shopee/Lazada |
| 1TB SSD | 1TB | ₱3,500-₱5,000 | Shopee/Lazada |
| 2TB HDD (internal) | 2TB | ₱3,000-₱4,000 | Computer City |

---

## Stress Test

Now let's prove your homelab is resilient:

1. **Stop all containers** — `docker compose down` (or `docker stop <name>` for each). Then run `docker start <name>` or `docker compose up -d`. They should come back up cleanly.
2. **Kill a container** — `docker kill uptime-kuma`. With `restart: unless-stopped`, it should come back automatically within a few seconds. Check with `docker ps`.
3. **Run Docker cleanup** — `docker system prune -f`. Check that your services are still running after. They should be — pruning removes unused resources, not running containers.
4. **Simulate a power outage** — `sudo reboot`. When your server comes back, SSH in and check `docker ps`. All your containers with `restart: unless-stopped` should be running automatically.

> **🔥 The Chaos Champion:** Set a container to `restart: always` instead of `unless-stopped`. Stop it manually (`docker stop <name>`), then wait — it will restart even though you explicitly stopped it. Now change it back to `unless-stopped` (`docker update --restart unless-stopped <name>`), stop it again, and verify it stays stopped. This teaches you the difference between the two policies.

---

## What's Next

Your services are running, they restart automatically, and you have health monitoring in place. In Chapter 6, we'll explore your network — how devices talk to each other, how to make your services accessible across your home, and the fundamentals of networking that every homelabber needs to understand.

**Homework:**
1. Verify your auto-restart policy is set on all containers
2. Set up monitors in Uptime Kuma for all your services
3. Run `docker system df` to see Docker's disk usage
4. Do a manual backup of your service data (copy to ~/homelab-backup/)
5. Reboot your server and verify everything comes back up

> **🚀 Turbo:** Learn `docker compose ps` — it shows all services, their status, restart count, and uptime. Combined with `docker stats` (real-time CPU/memory), you'll know exactly what's happening in your homelab at any moment. Bonus: try `docker events` to see live Docker events as they happen.

---

## Go Deeper

- [Docker Restart Policies](https://docs.docker.com/reference/cli/docker/container/run/#restart) — Official docs
- [Uptime Kuma Documentation](https://github.com/louislam/uptime-kuma) — Monitoring setup
- [Docker System Prune](https://docs.docker.com/reference/cli/docker/system/prune/) — Disk cleanup reference
- [apcupsd Documentation](https://apcupsd.github.io/apcupsd/) — UPS management
- [Restic Documentation](https://restic.readthedocs.io/) — Advanced backup tool (Chapter 9)

> **💸 Lean Path:** All the tools in this chapter are free: Docker restart policies (built in, free), Uptime Kuma (open source, free), `docker system prune` (built in, free), and basic rsync backups (built into Linux, free). Even the UPS recommendation is optional — your homelab will survive without one. The monitoring stack in Chapter 11 (Prometheus + Grafana) is also completely free. You're building enterprise-grade reliability with ₱0 in software costs.
