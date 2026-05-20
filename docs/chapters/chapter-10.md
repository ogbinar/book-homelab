# Chapter 10: Automation — Work Less

## What You'll Build
Automated systems that keep your homelab running: automatic container updates, scheduled maintenance scripts, and cron jobs for common tasks. You'll go from manually managing your homelab to a system that mostly manages itself.

## How Long It Takes
2-3 hours (including reading, scripting, and testing)

## What You Need
- Your homelab stack running (Chapters 1-9)
- Basic familiarity with bash scripting

---

## The Story

Right now, your homelab works. But working isn't the same as maintained.

Every few weeks, new Docker images come out with security fixes. Every month, you need to check your backup logs. Every day, your disk fills up a little more. If you're manually doing all of this, you're not building a homelab — you're working a second job. *Gusto mo ba ng second job na walang bayad? Exactly. That's why we automate.*

**Automation is the difference between a hobby and an infrastructure.**

Think of automation like a dishwasher. You could wash every dish by hand. It works. But why would you, when a machine can do it while you do something else?

We're going to set up the homelab equivalent of a dishwasher. *Hindi ka nagluluto ng pagkain para maghugas ng plato, right? So why spend your time on repetitive tasks na pwedeng gawin ng machine?*

---

## Why This Matters

Automation isn't about being lazy (though that helps). It's about:

1. **Consistency** — Scripts do the same thing every time. Humans don't.
2. **Reliability** — Automated updates catch security fixes you'd otherwise forget.
3. **Time** — The hours you save automating compound over months and years.
4. **Documentation** — A script is executable documentation of what you do.

> **📢 Jargon Alert:** "Cron" — A time-based job scheduler in Unix-like operating systems. Think of it as a calendar that runs commands for you. "At 3 AM every day, run this script."

---

## 🟢 Quick Start

### Step 1: Automatic Container Updates with Watchtower

[Watchtower](https://github.com/containrrr/watchtower) automatically updates your Docker containers when new images are available.

**Add to your docker-compose.yml:**
```yaml
  watchtower:
    image: containrrr/watchtower:latest
    container_name: watchtower
    restart: unless-stopped
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      # Update once per day at 4 AM
      - WATCHTOWER_SCHEDULE=0 0 4 * * *
      # Cleanup old images after update
      - WATCHTOWER_CLEANUP=true
      # Only update containers with the label
      - WATCHTOWER_LABEL_ENABLE=true
      # Email notifications (optional)
      - WATCHTOWER_NOTIFICATIONS=shoutrrr
      - WATCHTOWER_NOTIFICATION_URL=telegram://BOT_TOKEN@telegram?channels=CHANNEL_NAME
    labels:
      - "com.centurylinklabs.watchtower.enable=true"
```

> **⚠️ Watch Out:** Watchtower will restart your containers automatically. This causes brief downtime (seconds to minutes). Schedule it for a time when downtime is acceptable.

### Step 2: Disk Usage Monitoring Script

```bash
nano ~/homelab-monitor.sh
```

```bash
#!/bin/bash
# homelab-monitor.sh — Daily homelab health check

REPORT="Homelab Daily Report - $(date +%Y-%m-%d)"

echo "═══════════════════════════════════════" >> /home/your-username/homelab-status.log
echo "$REPORT" >> /home/your-username/homelab-status.log
echo "═══════════════════════════════════════" >> /home/your-username/homelab-status.log

# Disk usage
echo "" >> /home/your-username/homelab-status.log
echo "📊 Disk Usage:" >> /home/your-username/homelab-status.log
df -h / | tail -1 >> /home/your-username/homelab-status.log

# Docker disk usage
echo "" >> /home/your-username/homelab-status.log
echo "🐳 Docker Disk Usage:" >> /home/your-username/homelab-status.log
docker system df >> /home/your-username/homelab-status.log

# Running containers
echo "" >> /home/your-username/homelab-status.log
echo "📦 Running Containers:" >> /home/your-username/homelab-status.log
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}" >> /home/your-username/homelab-status.log

# Uptime
echo "" >> /home/your-username/homelab-status.log
echo "⏱ Server Uptime:" >> /home/your-username/homelab-status.log
uptime >> /home/your-username/homelab-status.log

# Memory
echo "" >> /home/your-username/homelab-status.log
echo "💾 Memory Usage:" >> /home/your-username/homelab-status.log
free -h | grep Mem >> /home/your-username/homelab-status.log

# Check for failed containers
echo "" >> /home/your-username/homelab-status.log
echo "❌ Failed Containers:" >> /home/your-username/homelab-status.log
FAILED=$(docker ps -a --filter "status=exited" --format "{{.Names}} ({{.Status}})")
if [ -z "$FAILED" ]; then
  echo "  None — all healthy ✓" >> /home/your-username/homelab-status.log
else
  echo "$FAILED" >> /home/your-username/homelab-status.log
fi

echo "" >> /home/your-username/homelab-status.log
echo "$(date) — Report complete" >> /home/your-username/homelab-status.log
```

Make it executable:
```bash
chmod +x ~/homelab-monitor.sh
```

Schedule it to run daily:
```bash
crontab -e
```

Add:
```
0 6 * * * /home/your-username/homelab-monitor.sh
```

### Step 3: Docker Cleanup Script

```bash
nano ~/homelab-cleanup.sh
```

```bash
#!/bin/bash
# homelab-cleanup.sh — Monthly Docker cleanup

echo "Starting Docker cleanup..."

# Remove stopped containers
echo "Removing stopped containers..."
docker container prune -f

# Remove unused images
echo "Removing unused images..."
docker image prune -a -f --filter "until=168h"

# Remove unused volumes (BE CAREFUL — this removes unreferenced volumes)
echo "Removing unused volumes..."
docker volume prune -f --filter "label!=keep"

# Remove build cache
echo "Removing build cache..."
docker builder prune -f

echo "Cleanup complete!"
echo "Saved: $(docker system df --format '{{.Size}}' | head -1)"
```

Schedule monthly:
```bash
crontab -e
```

Add:
```
0 5 1 * * /home/your-username/homelab-cleanup.sh >> /home/your-username/cleanup.log 2>&1
```

> **⚠️ Watch Out:** `docker volume prune` removes volumes that are not used by any container. Make sure you're not losing important data. The `--filter "label!=keep"` approach lets you mark important volumes with a `keep` label to protect them.

### Step 4: Automated Service Restart Script

Sometimes containers get stuck or memory leaks accumulate. A weekly restart keeps things clean:

```bash
nano ~/homelab-restart.sh
```

```bash
#!/bin/bash
# homelab-restart.sh — Weekly service restart
# Runs every Sunday at 5 AM

cd /home/your-username/homelab

echo "[$(date)] Starting scheduled restart..."

# Graceful restart
docker compose down
sleep 5
docker compose up -d

echo "[$(date)] Restart complete."
```

Schedule it:
```bash
crontab -e
```

Add:
```
0 5 * * 0 /home/your-username/homelab-restart.sh >> /home/your-username/restart.log 2>&1
```

---

## 🔵 The Why

### What Each Automation Does

| Automation | Frequency | Purpose |
|---|---|---|
| **Watchtower** | Daily (4 AM) | Pull and apply Docker image updates |
| **Health monitor** | Daily (6 AM) | Log system status for quick review |
| **Cleanup** | Monthly (1st) | Remove stale images, containers, and volumes |
| **Restart** | Weekly (Sunday 5 AM) | Clear memory leaks, apply kernel updates |
| **Backup** | Daily (3 AM) | Copy data to external drive |

### The Philosophy: Automate the Boring, Manual the Important

Not everything should be automated:
- ✅ **Should automate:** Updates, backups, cleanup, monitoring, restarts
- ❌ **Should NOT automate:** Security configuration, backup verification, disaster recovery

The boring stuff (updating images, cleaning up disk space) should run automatically. The important stuff (is this backup actually working? did the security update break anything?) should be reviewed manually.

### Cron Schedule Explained

Cron uses this format: `minute hour day-of-month month day-of-week command`

```
0 3 * * *    → Every day at 3:00 AM
0 4 * * *    → Every day at 4:00 AM
0 5 * * 0    → Every Sunday at 5:00 AM
0 0 1 * *    → First day of every month at midnight
*/30 * * * * → Every 30 minutes
```

---

## 🟣 Deep Dive

### Advanced Monitoring with Telegram

Get notifications when things break:

```bash
# Create a Telegram bot (talk to @BotFather on Telegram)
# Get your BOT_TOKEN and CHANNEL_NAME

# Add to docker-compose.yml
  notifications:
    image: containrrr/watchtower:latest
    environment:
      - WATCHTOWER_NOTIFICATIONS=shoutrrr
      - WATCHTOWER_NOTIFICATION_URL=telegram://BOT_TOKEN@telegram?channels=CHANNEL_NAME
```

Or use a simpler approach with curl:

```bash
# Add to homelab-monitor.sh, after the report:
curl -s -X POST "https://api.telegram.org/BOT_TOKEN/sendMessage" \
  -d "chat_id=CHANNEL_ID" \
  -d "text=$REPORT" \
  -d "parse_mode=HTML"
```

### Docker Label-Based Watchtower

Instead of updating ALL containers, use labels to control which ones Watchtower updates:

```yaml
services:
  my-service:
    image: my-service:latest
    labels:
      - "com.centurylinklabs.watchtower.enable=true"  # Will update
  stable-service:
    image: stable-service:v2.1
    labels:
      - "com.centurylinklabs.watchtower.enable=false"  # Won't update
```

This prevents Watchtower from updating services on pinned versions that you don't want to change.

---

## 💼 Career Boost

**What you learned that employers care about:**
- Infrastructure automation
- Scheduled task management (cron)
- System monitoring and alerting
- Container lifecycle automation
- Maintenance procedures

**Interview talking point:**
> *"I designed an automation framework for my homelab infrastructure using Watchtower for container updates, cron-scheduled health monitoring, and automated cleanup procedures. This reduced manual maintenance time from 4 hours/week to 30 minutes/week while improving system reliability."*

---

## 🇵🇭 PH Context

> **💸 Lean Path:** All the automation tools in this chapter are free: Watchtower (open source), cron (built into Linux), bash scripts (free), Telegram bot (free with @BotFather). You're not paying for any monitoring or automation software — just your time to set it up once and it runs forever.

### When Power Goes Out

Automation scripts won't run if the power is out. When power returns:

1. **Server boots** → Docker starts (if `restart: unless-stopped`)
2. **Watchtower** → Won't run until its cron schedule fires
3. **Backups** → Won't run until 3 AM

**Solution:** Add a systemd service that triggers backup after boot:

```bash
# /etc/systemd/system/homelab-boot-backup.service
[Unit]
Description=Homelab backup after boot
After=docker.service network-online.target
Requires=docker.service

[Service]
Type=oneshot
ExecStart=/home/your-username/homelab-backup.sh

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl enable homelab-boot-backup.service
```

### Internet Dependency

Watchtower and Telegram notifications require internet. If your internet is down (PLDT fiber cut, Globe line maintenance, or typhoon season — we've all experienced it):

- Telegram notifications can't send (harmless — you'll see them when internet returns)
- Watchtower can't check for updates (harmless — your services keep running)
- Automated scripts that rely on external APIs will fail (check your error logs)
**The key insight:** Your homelab services themselves should work locally even when the internet is down. Watchtower and Telegram are convenience features — not core services.
- Local automation (cleanup, restart) works fine

---

## Stress Test

Now let's prove your automation works:

1. **Delete all your Docker images** (`docker image prune -a -f`). Then run `docker compose up -d`. Docker will pull all images again. This proves your compose file is complete and correct.
2. **Kill Watchtower mid-update** (`docker rm -f watchtower`). Then start it again. The containers it was updating should resume normally. This proves Watchtower is safe to interrupt.
3. **Delete your monitoring script** (`rm ~/homelab-monitor.sh`). Then recreate it from memory. If you can't, your automation isn't truly yours — you just copied a script.
4. **Break your crontab** — edit it (`crontab -e`), add a bad entry, save. Then fix it. This proves you understand cron syntax well enough to debug it.

> **🔥 The Chaos Champion:** Comment out ALL your cron jobs. Wait 24 hours. Check your backup logs, your monitor logs, your disk usage. Everything that should have run but didn't — that's the gap between "automated" and "reliably automated." The fix isn't more automation. It's monitoring the automation itself.

---

## What's Next

Your homelab is now largely automated. It updates itself, backs up itself, and monitors itself. In Chapter 11, we'll add professional-grade observability — because monitoring isn't just about knowing when things break. It's about understanding how your system behaves before it breaks.

**Homework:**
1. Set up Watchtower for automatic updates
2. Create and schedule the health monitoring script
3. Set up the monthly cleanup script
4. Add Telegram or email notifications for alerts

---

> **🚀 Turbo:** Want even more automation? Create a maintenance script that combines your health checks, cleanup, and backups into one daily routine. Schedule it with cron at 4 AM: clean Docker, verify backups, check disk space, update containers with Watchtower, and send a morning summary to Telegram. One script, zero daily maintenance. You wake up to a report, not an alert.

---

## Go Deeper

- [Watchtower Documentation](https://containrrr.dev/watchtower/) — Auto-update containers
- [Cron Documentation](https://man7.org/linux/man-pages/man5/crontab.5.html) — Schedule reference
- [systemd Documentation](https://www.freedesktop.org/software/systemd/man/systemd.service.html) — Service management
- [Prometheus + Grafana](https://prometheus.io/) — Professional monitoring (Chapter 11)
