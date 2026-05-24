# Chapter 9: Your Data, Safe

## What You'll Build
A complete backup strategy following the 3-2-1 rule: automated local backups to an external drive, plus optional offsite/cloud backup. You'll also test a full restore to prove everything works.

## How Long It Takes
2 hours (including reading, setup, and restore testing)

## What You Need
- Your homelab stack from Chapter 8
- An external USB drive (at least 2x your total data size)
- Optional: A cloud storage account (Google Drive, Backblaze B2, etc.)
- Basic backup from Chapter 5 (if you set one up)

---

## The Story

Here's a scenario: It's 2 AM. Your server crashes — maybe a power surge, maybe a bad update, maybe just old hardware giving up. You SSH in and find your filesystem corrupted. Your Docker containers are gone. Your Nextcloud files are gone. Your Vaultwarden password database — **gone**.

You can reinstall everything. But your **data**? The photos you took? The documents you stored? The 2,000 passwords you carefully migrated? Those are gone forever.

**Data loss is the one failure you cannot recover from.**

If you set up a quick manual backup in Chapter 5, you have a safety net. But a manual copy is not a strategy. *Ang manual backup parang nag-iipon ng pera sa ilalim ng kutson — may nakaipon ka, pero kailangan mong i-check kung nandoon pa rin.'* This chapter upgrades you from "I hope I backed up" to "I know my backups work" — with the full 3-2-1 strategy, automation, and tested restore drills.

That matters just as much in a Filipino home where the backup drive might live in a drawer, a closet, or a relative's place across town.

Think of this chapter as the water tank and pantry of the house. You are not just storing things. You are making sure the house can keep going when the faucet stops or the fridge goes empty.

---

## Why This Matters

The most common backup mistake is: **setting up backups but never testing them.**

A backup you haven't tested is not a backup. It's a prayer.

That's why restore drills matter: the real goal is not to have files somewhere. The real goal is to bring the house back when something breaks.

Testing restores is the difference between "I have a backup strategy" and "I have a working backup strategy."

> **📢 Jargon Alert:** "RPO" (Recovery Point Objective) — How much data can you afford to lose? If you backup daily, your RPO is 24 hours (you could lose up to a day's worth of data). If you backup every hour, your RPO is 1 hour.

> **📢 Jargon Alert:** "RTO" (Recovery Time Objective) — How long does it take to recover? If you can rebuild your homelab in 30 minutes, your RTO is 30 minutes.

---

## 🟢 Quick Start

### Step 1: The Simple Backup — rsync to External Drive

This is the easiest, most reliable backup method for homelabbers.

**Connect your external drive and find it:**
```bash
lsblk
```

Look for your external drive (e.g., `/dev/sdb1`). Mount it:
```bash
# Create mount point
sudo mkdir -p /media/backup

# Mount the drive
sudo mount /dev/sdb1 /media/backup

# Make it persist across reboots (get UUID first)
sudo blkid /dev/sdb1

# Add to /etc/fstab (replace UUID with yours)
echo 'UUID=your-uuid-here /media/backup ext4 defaults 0 2' | sudo tee -a /etc/fstab
```

### Step 2: Create a Backup Script

```bash
nano ~/homelab-backup.sh
```

```bash
#!/bin/bash
# homelab-backup.sh — Automated backup script
# Usage: ./homelab-backup.sh

set -e

# Configuration
BACKUP_MOUNT="/media/backup"
BACKUP_DIR="${BACKUP_MOUNT}/homelab-backup"
DATE=$(date +%Y-%m-%d_%H%M%S)
RETENTION_DAYS=30
LOG_FILE="${BACKUP_MOUNT}/backup.log"
SERVICES_DIR="$HOME/homelab"

# Timestamp
echo "[$(date)] Starting backup..." >> "$LOG_FILE"

# Ensure backup directory exists
mkdir -p "${BACKUP_DIR}/${DATE}"

# Backup Docker volumes using Docker
cd "$SERVICES_DIR"

# Backup each service's data volume
echo "[$(date)] Backing up Docker volumes..." >> "$LOG_FILE"

# Uptime Kuma
if docker volume inspect homelab_kuma-data >/dev/null 2>&1; then
  docker run --rm \
    -v homelab_kuma-data:/data:ro \
    -v "${BACKUP_DIR}/${DATE}:/backup" \
    alpine tar czf "/backup/kuma.tar.gz" -C /data . 2>>"$LOG_FILE"
  echo "[$(date)] ✓ Kuma backed up" >> "$LOG_FILE"
fi

# Vaultwarden
if docker volume inspect homelab_vault-data >/dev/null 2>&1; then
  docker run --rm \
    -v homelab_vault-data:/data:ro \
    -v "${BACKUP_DIR}/${DATE}:/backup" \
    alpine tar czf "/backup/vault.tar.gz" -C /data . 2>>"$LOG_FILE"
  echo "[$(date)] ✓ Vaultwarden backed up" >> "$LOG_FILE"
fi

# Nextcloud
if docker volume inspect homelab_nextcloud-data >/dev/null 2>&1; then
  docker run --rm \
    -v homelab_nextcloud-data:/data:ro \
    -v "${BACKUP_DIR}/${DATE}:/backup" \
    alpine tar czf "/backup/nextcloud.tar.gz" -C /data . 2>>"$LOG_FILE"
  echo "[$(date)] ✓ Nextcloud backed up" >> "$LOG_FILE"
fi

# Configuration backup (docker-compose.yml, .env, Caddyfile)
echo "[$(date)] Backing up configuration..." >> "$LOG_FILE"
tar czf "${BACKUP_DIR}/${DATE}/config.tar.gz" \
  -C "$SERVICES_DIR" \
  docker-compose.yml .env config/ 2>>"$LOG_FILE"
echo "[$(date)] ✓ Configuration backed up" >> "$LOG_FILE"

# Clean old backups (keep last 30 days)
echo "[$(date)] Cleaning old backups..." >> "$LOG_FILE"
find "${BACKUP_DIR}" -maxdepth 1 -type d -mtime +${RETENTION_DAYS} -exec rm -rf {} \; 2>>"$LOG_FILE"

echo "[$(date)] Backup complete: ${DATE}" >> "$LOG_FILE"
echo "Backup saved to: ${BACKUP_DIR}/${DATE}"
```

Make it executable:
```bash
chmod +x ~/homelab-backup.sh
```

### Step 3: Test the Backup

```bash
~/homelab-backup.sh
```

Check the output:
```bash
ls -la /media/backup/homelab-backup/
```

You should see a dated directory with backup files.

### Step 4: Schedule Automatic Backups

```bash
crontab -e
```

Add this to run daily at 3 AM:
```
0 3 * * * /home/your-username/homelab-backup.sh >> /home/your-username/backup-cron.log 2>&1
```

### Step 5: Test a Restore (The Most Important Step)

This is non-negotiable. A backup you haven't tested is worthless.

```bash
# Pick your latest backup
LATEST=$(ls -td /media/backup/homelab-backup/*/ | head -1)
echo "Restoring from: $LATEST"

# Create a test directory
mkdir -p /tmp/restore-test

# Extract the backup
tar xzf "${LATEST}kuma.tar.gz" -C /tmp/restore-test

# Verify the contents
ls -la /tmp/restore-test

# Check file sizes (should not be 0)
du -sh /tmp/restore-test
```

**Full restore drill:**
1. Stop all services: `docker compose down`
2. Delete your data: `rm -rf ~/homelab/data/*`
3. Restore from backup: `tar xzf $LATEST/kuma.tar.gz -C ~/homelab/data/kuma/`
4. Restart: `docker compose up -d`
5. Verify in browser: services should work exactly as before

> **🔥 The Chaos Champion:** Do this restore drill now. It takes 20 minutes. It will feel unnecessary. But trust me — when your disk fails (and it will), you will be incredibly grateful you did this today.

---

## 🔵 The Why

### The 3-2-1 Backup Rule, Applied

| Rule | Homelab Implementation |
|---|---|
| **3** copies of data | Live data + external drive + cloud |
| **2** different media | Internal drive + external USB drive |
| **1** offsite | Cloud backup or drive at a friend's house |

### Backup Options for Offsite

| Option | Cost | Complexity | Best For |
|---|---|---|---|
| **External drive at friend's** | ₱0 | Low | Everyone |
| **Backblaze B2** | ~₱150/month (100GB) | Medium | Automated cloud backup |
| **Google Drive** | 15GB free | Low | Small backups |
| **rsync to remote server** | Cost of remote server | High | Advanced users |

> **💸 Lean Path:** You don't need a cloud backup service. A ₱2,000 external drive (1TB on Shopee) gives you the "3-2-1" rule locally: 3 copies (live + external + friend's house), 2 media types (internal + external drive), 1 offsite (friend's house or a USB drive in a safety deposit box). Cloud services add recurring cost — local backups are one-time.

### Using Restic for Advanced Backups

Once you're comfortable with basic backups, try [Restic](https://restic.net/) — an encrypted, deduplicated backup tool that supports cloud storage:

```bash
# Install Restic
sudo apt install restic

# Initialize a repository (on your external drive)
restic init --repo /media/backup/restic-backup

# Backup your homelab
restic backup --repo /media/backup/restic-backup ~/homelab/data

# List backups
restic snapshots --repo /media/backup/restic-backup

# Restore from backup
restic restore latest --repo /media/backup/restic-backup --target /tmp/restore
```

**Restic advantages:**
- Encrypted backups (your data is safe even if the drive is stolen)
- Deduplication (saves space)
- Incremental backups (only changes are stored)
- Cloud support (S3, Azure, Backblaze B2, etc.)

---

## 🟣 Deep Dive

### Backup Automation with Docker

For a more Docker-native approach, use a dedicated backup container:

```yaml
# Add to your docker-compose.yml
backup:
  image: restic/restic:latest
  container_name: homelab-backup
  restart: "no"
  volumes:
    - ./data:/backups/data:ro
    - ./backup:/backups/backup
    - ./restic-cache:/backups/cache
  environment:
    - RESTIC_REPOSITORY=/backups/backup
    - RESTIC_PASSWORD=${RESTIC_PASSWORD:-changeme}
    - RESTIC_CACHE_DIR=/backups/cache
  command: backup --tag homelab /backups/data
  entrypoint: sh -c
```

Run manually: `docker compose run --rm backup`
Schedule: Use a cron container or systemd timer.

### Backup Verification Strategy

Don't just trust that backups work. Verify regularly:

```bash
# Monthly verification script
nano ~/verify-backup.sh
```

```bash
#!/bin/bash
# verify-backup.sh — Monthly backup verification

LATEST=$(ls -td /media/backup/homelab-backup/*/ | head -1)
TEST_DIR="/tmp/backup-verify-$(date +%Y%m%d)"

mkdir -p "$TEST_DIR"

# Verify each backup file can be extracted
for backup in "$LATEST"/*.tar.gz; do
  if tar tzf "$backup" >/dev/null 2>&1; then
    echo "✓ $(basename $backup) — valid"
  else
    echo "✗ $(basename $backup) — CORRUPTED!"
  fi
done

# Clean up
rm -rf "$TEST_DIR"
```

Run this monthly and note the results.

---

## 💼 Career Boost

**What you learned that employers care about:**
- Disaster recovery planning
- Backup strategy design and implementation
- Data integrity verification
- Automated backup scheduling
- Recovery Time/Objective planning

**Interview talking point:**
> *"I implemented a comprehensive backup strategy for my homelab infrastructure using the 3-2-1 rule: automated daily rsync backups to external storage with 30-day retention, monthly restore verification drills, and encrypted Restic backups for offsite redundancy. Achieved RPO of 24 hours and RTO of 30 minutes."*

---

## 🇵🇭 PH Context

### Where to Buy External Drives

| Store | Price Range (1TB) | Notes |
|---|---|---|
| Shopee | ₱2,000-₱3,000 | Check seller ratings |
| Lazada | ₱2,200-₱3,200 | Often has vouchers |
| Computer City | ₱2,500-₱3,500 | Can test before buying |
| Abenson | ₱2,800-₱4,000 | Warranty included |

### Flood Consideration

If you live in a flood-prone area (common in Metro Manila and other PH cities):
- Keep your external drive in a waterproof container
- Consider a higher shelf or cabinet
- The "friend's house" offsite copy isn't just for digital disasters — it's for physical ones too

### Power Considerations for Backups

If your backup runs during a power outage:
- The external drive won't spin up
- The backup will fail silently
- Check your backup logs regularly: `cat /media/backup/backup.log`

### Internet Outages and Cloud Backups

When your ISP goes down (PLDT fiber cuts, Globe maintenance, typhoon damage to lines — we've all seen it happen), cloud-based backups will fail. This is why the 3-2-1 rule matters: local backups work even when the internet is dead.

**If you use cloud backups (Backblaze B2, Google Drive, etc.):**
- Set your backup schedule to run during off-peak hours (when internet is more likely to be up)
- Configure retry logic — Restic and BorgBackup both handle this automatically
- Don't panic if a backup misses — the next scheduled run will catch up
- Keep at least one local backup as your primary recovery path

---

## Stress Test

Now let's prove your backups actually work:

1. **Pick a file, delete it, restore from backup** — Choose a non-critical file in `~/homelab/data/`, note its name, then delete it. Restore it from your latest backup: `tar xzf $(ls -td /media/backup/homelab-backup/*/ | head -1)kuma.tar.gz -C /tmp/restore-verify && ls /tmp/restore-verify`. Verify the file is there.
2. **Verify the 3-2-1 rule** — Do you have 3 copies of your data? (live + external drive + offsite). Are they on 2 different media types? (internal drive + external USB). Is 1 copy offsite? (friend's house, cloud, or USB in a safety deposit box). If any answer is no, fix it now.
3. **Schedule a monthly restore drill** — Add a calendar reminder: "Homelab Backup Test" on the first Sunday of each month. Pick a service, delete its data directory, and restore from backup. Time yourself. Your goal: under 30 minutes.
4. **Test with the external drive disconnected** — Unplug your backup drive. Run your backup script. Does it fail gracefully? Check the log. A backup that silently fails is worse than no backup at all.

> **🔥 The Chaos Champion:** Do a full restore drill now. Stop all services, delete `~/homelab/data/*`, restore from your latest backup, restart everything, and verify in the browser. It takes 20 minutes. It will feel unnecessary. But trust me — when your disk fails (and it will), you will be incredibly grateful you did this today.

---

## What's Next

Your data is backed up and verified. In Chapter 10, we'll automate the boring stuff — updating containers, managing updates, and setting up scripts so you spend less time maintaining and more time using your homelab.

**Homework:**
1. Set up automated daily backups to an external drive
2. Test a full restore from backup
3. Schedule monthly backup verification
4. Set up one offsite backup (friend's house or cloud)

---

> **🚀 Turbo:** Want encrypted backups without manual key management? Set up Restic with a key file: `export RESTIC_PASSWORD_FILE=~/.restic-key` (store your password in a file, not the shell). Then configure Restic to back up to multiple destinations simultaneously — local drive AND cloud — using `restic repo add`. This gives you true 3-2-1 automation: one command, multiple destinations, all encrypted.

---

## Go Deeper

- [Restic Documentation](https://restic.readthedocs.io/) — Advanced backup tool
- [BorgBackup Documentation](https://www.borgbackup.org/) — Alternative deduplicating backup
- [Kopia Documentation](https://kopia.io/) — Backup with GUI support
- [3-2-1 Backup Strategy](https://www.backblaze.com/blog/the-3-2-1-backup-strategy/) — Backblaze's guide
