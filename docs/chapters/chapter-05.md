# Chapter 5: Keeping It Alive

## What You'll Build
A resilient homelab that survives reboots, crashes, and power outages. You'll set up auto-restart policies, automated backups, health checks, and update procedures.

## How Long It Takes
1 hour (including reading and hands-on steps)

## What You Need
- Your server from Chapter 2 with Docker and at least one service running (Chapter 4)
- An external USB drive or secondary storage (for backups)
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

### Step 2: Set Up Automated Backups

Data is more important than services. Services can always be redeployed. Data — your photos, your passwords, your documents — once it's gone, it's gone.

We'll use a simple but effective backup strategy: copy your Docker volumes to an external drive.

**If you have an external USB drive:**

```bash
# Create a backup script
nano ~/homelab-backup.sh
```

Paste this:

```bash
#!/bin/bash
# ~/homelab-backup.sh — Simple homelab backup script

# Configuration
BACKUP_DIR="/media/usb-backup/homelab-backup"
DATE=$(date +%Y-%m-%d_%H%M%S)
RETENTION_DAYS=30
SERVICES="uptime-kuma vaultwarden nextcloud pihole jellyfin"

# Create backup directory
mkdir -p "${BACKUP_DIR}/${DATE}"

# Backup each service's data
for service in $SERVICES; do
  # Check if the container exists
  if docker ps -a --format '{{.Names}}' | grep -q "^${service}$"; then
    echo "Backing up ${service}..."
    docker run --rm \
      -v "${service}_data:/data:ro" \
      -v "${BACKUP_DIR}/${DATE}:/backup" \
      alpine tar czf "/backup/${service}.tar.gz" -C /data . 2>/dev/null || \
    echo "  ⚠ Could not backup ${service} (volume may not exist)"
  fi
done

# Clean old backups
find "${BACKUP_DIR}" -maxdepth 1 -type d -mtime +${RETENTION_DAYS} -exec rm -rf {} \;

echo "Backup complete: ${DATE}"
```

Make it executable:
```bash
chmod +x ~/homelab-backup.sh
```

Test it:
```bash
~/homelab-backup.sh
```

**How to mount an external USB drive on Ubuntu:**
```bash
# Find the USB drive
lsblk

# Create a mount point
sudo mkdir -p /media/usb-backup

# Mount it (replace sdb1 with your actual drive partition)
sudo mount /dev/sdb1 /media/usb-backup

# Make it persist across reboots
# Get the UUID
sudo blkid /dev/sdb1

# Add to fstab (replace UUID with your actual one)
echo 'UUID=your-uuid-here /media/usb-backup ext4 defaults 0 2' | sudo tee -a /etc/fstab
```

### Step 3: Schedule Automatic Backups

Use `cron` to run your backup script automatically:

```bash
# Edit your crontab
crontab -e
```

Add this line to run backups daily at 3 AM:
```
0 3 * * * /home/your-username/homelab-backup.sh >> /home/your-username/backup.log 2>&1
```

Replace `your-username` with your actual username.

**Verify it's scheduled:**
```bash
crontab -l
```

> **💡 Quick Win:** Even if you skip everything else in this chapter, setting up automated backups is the single most important thing you can do. Data loss is the one failure you can't recover from.

### Step 4: Test Your Backup (The Most Important Step)

A backup you haven't tested is just a hope. Let's verify you can actually restore:

```bash
# Find your latest backup
ls -la ~/homelab-backup/homelab-backup/

# Extract the latest backup to a test location
LATEST=$(ls -td ~/homelab-backup/homelab-backup/*/ | head -1)
mkdir -p /tmp/backup-test
tar xzf "${LATEST}uptime-kuma.tar.gz" -C /tmp/backup-test

# Check the contents
ls -la /tmp/backup-test
```

If you can see files in `/tmp/backup-test`, your backup works.

**The restore drill:**
1. Delete your container: `docker rm -f uptime-kuma`
2. Clean your data: `rm -rf ~/uptime-kuma/data/*`
3. Restore from backup: `tar xzf ~/homelab-backup/homelab-backup/LATEST/uptime-kuma.tar.gz -C ~/uptime-kuma/data/`
4. Recreate the container (same command from Chapter 3)
5. Verify it works in your browser

> **🔥 Stress Test:** Do this restore drill. It takes 15 minutes. It might feel unnecessary now. But when something actually breaks (and it will), you'll be the person who smiles because you already know how to fix it.

### Step 5: Set Up Health Monitoring

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

---

## 🔵 The Why

### The 3-2-1 Backup Rule

Professional data protection follows the **3-2-1 rule**:
- **3** copies of your data (original + 2 backups)
- **2** different types of storage (e.g., internal drive + external drive)
- **1** copy offsite (e.g., cloud storage, friend's house, USB drive at office)

For homelabbers, this looks like:
- **Copy 1:** Your live data on the server
- **Copy 2:** Your external USB drive (local backup)
- **Copy 3:** Optional — sync to a cloud service or another location

> **💸 The Lean Path:** If you don't have an external drive yet, your backup is "manual: copy important data to a friend's house or Google Drive." That's better than nothing. We're building toward automation, but starting is what matters.

### Why Auto-Restart Matters

Without auto-restart, every container crash means:
1. You notice something's down (maybe)
2. You SSH into your server
3. You check the logs
4. You start the container manually
5. You hope you did it right

With auto-restart, steps 1-5 happen automatically. You only get involved when something truly needs your attention.

### Why Testing Backups Matters

Studies show that **60% of organizations with a backup strategy discover it doesn't work** when they need it. Common reasons:
- Corrupted backup files
- Missing files in the backup
- Backup process silently failing
- Restoring to incompatible hardware/software

Testing proves your backup works. Not "probably works." Works.

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

Named volumes make backup scripts more predictable:
```bash
docker run --rm \
  -v kuma-data:/data:ro \
  -v /tmp/backup:/backup \
  alpine tar czf "/backup/uptime-kuma.tar.gz" -C /data .
```

### Backup Strategies Compared

| Strategy | Pros | Cons | Best For |
|---|---|---|---|
| **rsync to external drive** | Simple, fast, incremental | Manual setup | Beginners |
| **Docker volume backup** (above) | Container-aware, automated | Tar files aren't searchable | Docker-heavy setups |
| **Restic** | Encrypted, deduplicated, cloud support | Steeper learning curve | Advanced users |
| **BorgBackup** | Excellent deduplication, encrypted | Linux-only, complex | Large datasets |
| **Proxmox Backup Server** | VM/container-aware, incremental | Requires Proxmox | Virtualization users |

### Docker Cleanup

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

---

## 💼 Career Boost

**What you learned that employers care about:**
- Disaster recovery planning
- Backup strategy design (3-2-1 rule)
- Automation with cron and scripting
- System reliability engineering
- Testing and validation procedures

**Interview talking point:**
> *"I implemented a 3-2-1 backup strategy for my homelab infrastructure, with automated daily backups verified through monthly restore drills. I use cron-scheduled scripts to maintain 30 days of backup history, achieving RPO (Recovery Point Objective) of 24 hours and RTO (Recovery Time Objective) of 15 minutes."*

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

## What's Next

Your services are running, they restart automatically, and your data is backed up. In Chapter 6, we'll explore your network — how devices talk to each other, how to make your services accessible across your home, and the fundamentals of networking that every homelabber needs to understand.

**Homework:**
1. Verify your auto-restart policy is set on all containers
2. Run your backup script and verify the output
3. Practice a restore from backup
4. Set up cron for automatic daily backups
5. Add monitors in Uptime Kuma for all your services

---

## Go Deeper

- [3-2-1 Backup Strategy Explained](https://www.backblaze.com/blog/the-3-2-1-backup-strategy/) — Backblaze's guide
- [Docker Restart Policies](https://docs.docker.com/reference/cli/docker/container/run/#restart) — Official docs
- [Cron Documentation](https://man7.org/linux/man-pages/man5/crontab.5.html) — Linux cron reference
- [Restic Documentation](https://restic.readthedocs.io/) — Advanced backup tool
- [apcupsd Documentation](https://apcupsd.github.io/apcupsd/) — UPS management
