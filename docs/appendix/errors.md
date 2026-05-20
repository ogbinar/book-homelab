# Common Errors

_The top 50+ errors you'll encounter, with causes, solutions, and Philippines-specific context._

> **Quick Reference:** Most errors are covered in **[Chapter 3](chapters/chapter-03.md)** (Docker basics), **[Chapter 5](chapters/chapter-05.md)** (keeping services alive), **[Chapter 7](chapters/chapter-07.md)** (reverse proxy + remote access).

---

## How to Use This Appendix

Each error entry follows this format:

```
ERROR: [exact error message]
CAUSE: [plain English explanation]
SOLUTION: [step-by-step fix]
PH CONTEXT: [ISP-specific or local infrastructure note]
```

Use the table of contents below to jump to your error category, or search (Ctrl+F) for keywords.

---

## Table of Contents

- [Docker Errors (15)](#docker-errors)
- [Proxmox Errors (10)](#proxmox-errors)
- [Linux/System Errors (13)](#linuxsystem-errors)
- [Network Errors (10)](#network-errors)
- [Service-Specific Errors (12)](#service-specific-errors)
- [The Universal Debugging Checklist](#the-universal-debugging-checklist)

---

## Docker Errors

### "port is already allocated"

**ERROR:** `docker: Error response from daemon: driver failed programming external connectivity on endpoint ... Bind for 0.0.0.0:8080 failed: port is already allocated.`

**CAUSE:** Another container or service is already using the port you're trying to map.

**SOLUTION:**
1. Find what's using the port: `sudo lsof -i :8080`
2. Stop the conflicting container: `docker stop <container-name>`
3. Or use a different port in your docker-compose.yml or docker run command

**PH CONTEXT:** If you're running multiple homelabs or services from different guides, port conflicts are the #1 issue. Keep a running list: Kuma=3001, Vault=8080, Nextcloud=8081, Jellyfin=8096, Pi-hole=8082, Caddy=80/443, Prometheus=9090, Grafana=3000.

---

### "no space left on device"

**ERROR:** `docker: no space left on device` or `Write failed` errors during `docker pull` or `docker run`.

**CAUSE:** Your disk is full. Docker needs free space for new images, container layers, and logs.

**SOLUTION:**
1. Check disk usage: `df -h`
2. Check Docker usage: `docker system df`
3. Quick cleanup: `docker system prune -a --volumes` (⚠️ removes EVERYTHING unused — be careful)
4. Safer cleanup: `docker system prune` (removes stopped containers, unused networks, dangling images)
5. Remove specific images: `docker image prune -a`
6. For logs eating space: `sudo truncate -s 0 /var/log/syslog`

**PH CONTEXT:** If you're on mobile data, pulling large Docker images can consume a lot of bandwidth. Pull images on WiFi when possible. Some Philippine ISPs also have data caps on certain plans.

---

### "container exited with non-zero exit code"

**ERROR:** `docker ps` shows a container as `Exited (1)`, `Exited (137)`, or `Exited (139)`.

**CAUSE:** The application inside the container crashed.
- Exit code 1 = general error (bad config, missing dependency)
- Exit code 137 = killed by OOM (Out of Memory) — the system killed the container
- Exit code 139 = segmentation fault — the application accessed invalid memory

**SOLUTION:**
1. Check logs: `docker logs <container-name>`
2. If OOM (exit 137): Your server needs more RAM. Either upgrade RAM or use a lighter container.
3. If general error (exit 1): Read the logs carefully. Most container errors explain exactly what went wrong.
4. If segfault (exit 139): Update the container image — you may be running a buggy version.

**PH CONTEXT:** On low-RAM setups (4GB or less), OOM kills are extremely common when running multiple services. Start with 2-3 lightweight services and add more as you upgrade RAM.

---

### "unable to pull image" / rate limited

**ERROR:** `Error response from daemon: toomanyrequests: You have reached your pull rate limit.` or `Error: unable to pull image`

**CAUSE:** Docker Hub rate limiting. Free accounts can pull 100 images per 6 hours from the same IP.

**SOLUTION:**
1. Log in to Docker Hub: `docker login` (gives you a higher rate limit)
2. Wait and try again
3. Cache images before you need them: `docker pull <image>` during a quiet time
4. Use a mirror: Some Philippine networks benefit from Docker mirror proxies

**PH CONTEXT:** Docker Hub can be slow from the Philippines due to routing. If pulls are taking forever, try during off-peak hours (late night, early morning PH time).

---

### "permission denied" — Docker socket

**ERROR:** `Cannot connect to the Docker daemon. Is the docker daemon running?` or `permission denied while trying to connect to the Docker daemon socket`

**CAUSE:** Your user isn't in the `docker` group.

**SOLUTION:**
```bash
sudo usermod -aG docker $USER
newgrp docker
```
Then log out and log back in (or reboot).

**PH CONTEXT:** This is the first thing beginners miss. Always run this after installing Docker, before trying any `docker` commands.

---

### "volume already exists"

**ERROR:** `create <name>: "<name>" volume name is already in use`

**CAUSE:** A volume with the same name already exists from a previous container.

**SOLUTION:**
1. List volumes: `docker volume ls`
2. Remove unused volumes: `docker volume prune`
3. Or rename the volume in your docker-compose.yml
4. Or remove the specific volume: `docker volume rm <name>`

---

### "network already exists"

**ERROR:** `container failed to start - failed to create network: network ... already exists`

**CAUSE:** A Docker network with the same name already exists.

**SOLUTION:**
1. List networks: `docker network ls`
2. Remove unused networks: `docker network prune`
3. Or remove the specific network: `docker network rm <name>`

---

### "healthcheck failed"

**ERROR:** Container health status shows `unhealthy` in `docker ps`.

**CAUSE:** The health check command inside the container is failing. Could be a misconfiguration, resource constraints, or the service itself isn't ready yet.

**SOLUTION:**
1. Check health check logs: `docker inspect --format='{{json .State.Health}}' <container>`
2. Run the health check command manually inside the container: `docker exec <container> <healthcheck-command>`
3. If the service just needs more time, increase the health check timeout in your docker-compose.yml
4. If the service is genuinely broken, check regular logs: `docker logs <container>`

---

### "image manifest unknown"

**ERROR:** `manifest for <image> not found: manifest unknown`

**CAUSE:** The image tag you're requesting doesn't exist, or the image is for a different architecture.

**SOLUTION:**
1. Check available tags on Docker Hub
2. For Raspberry Pi (ARM): Make sure to use ARM-specific images or tags like `linux/arm64`
3. Specify the platform explicitly: `docker run --platform linux/arm64 <image>`
4. Use `latest` tag if a specific version doesn't exist

**PH CONTEXT:** Raspberry Pi users hit this most often. Always check if a Docker image supports ARM64 before pulling. Look for `linux/arm64` or `linux/arm/v7` in the image's manifest.

---

### "container is restarting"

**ERROR:** Container shows `Health: starting` or `Restarting (1) 5 seconds ago` in `docker ps`.

**CAUSE:** The container keeps crashing and Docker's restart policy keeps trying to bring it back up.

**SOLUTION:**
1. Stop the restart loop: `docker stop <container>` then `docker rm <container>`
2. Run manually to see the error: `docker run --rm <image>` (watch the output for the actual error)
3. Fix the underlying issue (config, permissions, missing env vars)
4. Recreate with `docker compose up -d`

**PH CONTEXT:** This happens when you copy-paste a docker-compose.yml without adjusting environment variables. Always check that your `.env` file matches the container's expected variables.

---

### "bind: address already in use"

**ERROR:** `Error starting userland proxy: listen tcp4 0.0.0.0:80: bind: address already in use`

**CAUSE:** A process on the host (not in Docker) is already using the port.

**SOLUTION:**
1. Find what's using the port: `sudo lsof -i :80` or `sudo ss -tlnp | grep :80`
2. Common culprits: Apache (`apache2`), Nginx (`nginx`), Caddy running outside Docker
3. Stop the conflicting service: `sudo systemctl stop apache2`
4. Or disable it: `sudo systemctl disable apache2`
5. Or use a different port

**PH CONTEXT:** If you installed Apache or Nginx from apt as part of a web development setup, they'll grab port 80 automatically. This is the most common port conflict on fresh Ubuntu installs.

---

### "docker: command not found"

**ERROR:** `bash: docker: command not found`

**CAUSE:** Docker isn't installed, or it's not in your PATH.

**SOLUTION:**
1. Install Docker: Follow the installation steps in Chapter 3
2. If using Docker Desktop on WSL2: `wsl --update && wsl --shutdown` then restart
3. Check if Docker is running: `sudo systemctl status docker`

---

### "invalid compose file"

**ERROR:** `ERROR: yaml: line X: mapping values are not allowed in this context`

**CAUSE:** Your docker-compose.yml has a YAML syntax error — usually wrong indentation or a missing colon.

**SOLUTION:**
1. Check the line number in the error message
2. YAML is sensitive to indentation — use spaces, NOT tabs
3. Validate your file: `docker compose config` (shows parsed config, highlights errors)
4. Common mistake: forgetting a colon after a key, or mixing tabs and spaces

---

### "volume mount failed"

**ERROR:** `ERROR: for <container> Cannot start service ... mount path does not exist`

**CAUSE:** The host directory you're trying to mount doesn't exist.

**SOLUTION:**
1. Create the directory: `mkdir -p ~/my-service/config`
2. Set permissions: `chmod -R 755 ~/my-service`
3. Recreate: `docker compose up -d`

**PH CONTEXT:** When copy-pasting paths, make sure the directory structure exists first. The `-p` flag in `mkdir -p` creates parent directories automatically.

---

## Proxmox Errors

### "VM creation failed — no storage available"

**ERROR:** `volume 'local-lvm:vm/100/vm-100-disk-0.qcow2' already exists` or `no storage available`

**CAUSE:** Either the VM ID is already in use, or your storage pool is full.

**SOLUTION:**
1. Check running VMs: `qm list`
2. Check storage: `df -h` or check in the Proxmox web UI
3. If storage is full: clean up old backups, expand the storage pool, or add a new disk
4. If VM ID conflict: delete the orphaned VM or use a different ID

---

### "VM won't start — QEMU killed by OOM"

**ERROR:** VM fails to start, and `dmesg | grep -i oom` shows OOM killer activity.

**CAUSE:** Proxmox host doesn't have enough RAM to allocate to the VM.

**SOLUTION:**
1. Check host RAM: `free -h`
2. Reduce VM RAM allocation in Proxmox UI or via `qm set <vmid> -memory <MB>`
3. Close unused VMs or containers on the host
4. Add more RAM to the host (best long-term solution)

**PH CONTEXT:** On budget hardware (8-16GB RAM), running even 2-3 VMs can exceed available memory. Use LXC containers instead of full VMs when possible — they use significantly less RAM.

---

### "network bridge not working"

**ERROR:** VMs can't reach the internet, or can't be reached from outside.

**SOLUTION:**
1. Check bridge config: `cat /etc/network/interfaces` — look for `auto vmbr0` and `bridge-interfaces eth0`
2. Restart networking: `systemctl restart networking`
3. Check Proxmox web UI → Datacenter → Your Node → Sysinfo → Network
4. Ensure your VM's network is set to `vmbr0` (not `vmbr1` or other)

**PH CONTEXT:** If your ISP uses CGNAT, your Proxmox VMs will share your host's CGNAT IP. They won't have unique public IPs. This is normal — use Cloudflare Tunnel or Tailscale for remote access.

---

### "disk io wait is high"

**ERROR:** System feels sluggish, `iostat` shows high `%iowait`.

**CAUSE:** Disk can't keep up with VM/container I/O requests.

**SOLUTION:**
1. Check which VM/container is using the most I/O: `iotop`
2. Move VMs from HDD to SSD storage
3. Increase disk I/O limits in Proxmox for non-critical VMs
4. Add more RAM (more cache = less disk reads)

---

### "storage backend not found"

**ERROR:** `storage 'xxxx' not found on node 'pve'`

**CAUSE:** A storage path configured in Proxmox doesn't exist on the filesystem.

**SOLUTION:**
1. Check storage path: `ls -la /path/to/storage`
2. Create the directory: `mkdir -p /path/to/storage`
3. Or fix the storage definition in Proxmox UI → Datacenter → Storage
4. For NFS mounts: check if the NFS server is reachable (`ping <nfs-server>`)

---

### "firmware update broke boot"

**ERROR:** Proxmox won't boot after a `apt update && apt upgrade`.

**SOLUTION:**
1. Boot from Proxmox ISO and try repair
2. If you have a backup: restore from backup
3. Check the Proxmox forum — this is rare but happens with kernel updates
4. **Prevention:** Always backup before major upgrades. Test upgrades on a non-production node first.

**PH CONTEXT:** Internet outages during `apt upgrade` can leave partial installs. If your connection drops mid-update, run `sudo dpkg --configure -a` to finish the pending configurations when you're back online.

---

### "LXC container template not found"

**ERROR:** `storage 'local' not found` or `template not available` when trying to create an LXC container.

**CAUSE:** The LXC template isn't downloaded for the selected storage.

**SOLUTION:**
1. Download templates: In Proxmox UI → Datacenter → Templates → download the template you need
2. Or via CLI: `pct update` after creating the container
3. Check available templates: `pct push` or check the Proxmox template list

---

### "ZFS pool won't mount"

**ERROR:** ZFS pool shows as `UNAVAIL` after reboot.

**SOLUTION:**
1. Import the pool: `zpool import -a`
2. Check pool status: `zpool status`
3. If a disk is missing: `zpool clear <poolname>`
4. Make sure the pool auto-imports on boot: `zpool set autostart=yes <poolname>`

**PH CONTEXT:** Power outages (brownouts) can corrupt ZFS pools. A UPS is essential if you're using ZFS. Without one, consider ext4 with regular backups instead.

---

### "CPU pinning causing hangs"

**ERROR:** VM or container becomes unresponsive after enabling CPU pinning.

**SOLUTION:**
1. Remove CPU pinning: In Proxmox UI → Options → CPU pinning → clear
2. Or via CLI: `qm set <vmid> -cpus 0` (0 = use all available cores)
3. Restart the VM
4. If you need pinning, pin to fewer cores and leave some available for the host

---

## Linux/System Errors

### "disk full but df shows plenty of space"

**ERROR:** `No space left on device` but `df -h` shows 50%+ free space.

**CAUSE:** All inodes are used up (many small files), OR deleted files are still held open by running processes.

**SOLUTION:**
1. Check inodes: `df -i`
2. If inodes are full: find directories with many files: `find / -xdev -printf '%h\n' | sort | uniq -c | sort -k 1 -n`
3. If deleted files hold space: `sudo lsof | grep deleted` — restart the holding process
4. Clean up log files: `sudo journalctl --vacuum-size=100M`

---

### "sudo: command not found"

**ERROR:** `sudo: command not found`

**CAUSE:** You're logged in as root (no sudo needed) or sudo isn't installed.

**SOLUTION:**
1. If you're root: just run the command without `sudo`
2. If sudo isn't installed: `apt install sudo`
3. Check your user: `whoami` — if it says `root`, you're already root

---

### "package installation failed — dependency error"

**ERROR:** `E: Unable to correct problems, you have held broken packages.`

**CAUSE:** A package requires another package that isn't available or conflicts with an installed package.

**SOLUTION:**
1. Fix dependencies: `sudo apt --fix-broken install`
2. Update package lists: `sudo apt update`
3. Try installing again: `sudo apt install <package>`
4. For held packages: `sudo apt unhold <package>`

**PH CONTEXT:** If your internet connection drops during `apt update`, your package lists may be inconsistent. Always run `sudo apt update && sudo apt upgrade` as a single command to avoid partial updates.

---

### "SSH disconnected — connection reset by peer"

**ERROR:** `Connection to <IP> closed by remote host` or `Connection reset by peer`

**CAUSE:** The server rebooted, the network dropped, or a firewall rule changed.

**SOLUTION:**
1. Check if the server is reachable: `ping <IP>`
2. If using Tailscale: check if Tailscale is running on both ends (`tailscale status`)
3. If you have physical access: check the server directly
4. If no physical access: check with your hosting provider or, for homelab, wait for the network to recover

**PH CONTEXT:** In the Philippines, brownouts cause sudden reboots. Configure your BIOS to "Power On After Power Loss" (varies by motherboard) so your server comes back up automatically.

---

### "systemd service won't start"

**ERROR:** `systemctl start <service>` fails with no clear error.

**SOLUTION:**
1. Check the service status: `systemctl status <service>`
2. Check logs: `journalctl -u <service> -e` (shows recent log entries)
3. Check the service file: `systemctl cat <service>`
4. Reload systemd: `sudo systemctl daemon-reload`
5. Try again: `sudo systemctl start <service>`

---

### "file permissions error"

**ERROR:** `Permission denied` when accessing a file or directory.

**SOLUTION:**
1. Check current permissions: `ls -la <file>`
2. Fix ownership: `sudo chown -R <user>:<group> <path>`
3. Fix permissions: `chmod -R 755 <path>` (for directories) or `chmod 644 <path>` (for files)
4. For Docker volumes: make sure the host directory has the right UID/GID matching the container user

---

### "time is wrong — SSL certificates fail"

**ERROR:** `SSL certificate problem: certificate has expired` or services reject connections due to cert errors.

**CAUSE:** Your server's clock is wrong. SSL certificates are time-sensitive.

**SOLUTION:**
1. Check current time: `date`
2. Enable NTP (time sync): `timedatectl set-ntp true`
3. Manually sync: `sudo ntpdate pool.ntp.org`
4. Install NTP daemon: `sudo apt install chrony` (recommended over ntp)

**PH CONTEXT:** Servers without a UPS may lose their BIOS clock after a brownout. If your time is wrong after a power outage, this is almost certainly the cause.

---

### "swap is too slow"

**ERROR:** System is sluggish, and `htop` shows high swap usage.

**CAUSE:** Your server is running out of RAM and using swap (disk-based memory), which is much slower.

**SOLUTION:**
1. Check swap usage: `swapon --show` and `free -h`
2. Add swap if needed: `sudo fallocate -l 2G /swapfile && sudo chmod 600 /swapfile && sudo mkswap /swapfile && sudo swapon /swapfile`
3. Make swap permanent: Add `/swapfile none swap sw 0 0` to `/etc/fstab`
4. Adjust swappiness: `sudo sysctl vm.swappiness=10` (lower = prefer RAM)
5. **Best fix:** Add more physical RAM

---

## Network Errors

### "can't resolve hostname"

**ERROR:** `Could not resolve hostname` or `ping: unknown host`

**CAUSE:** DNS configuration is wrong or the DNS server is unreachable.

**SOLUTION:**
1. Check DNS config: `cat /etc/resolv.conf`
2. Test with an IP: `ping 8.8.8.8` — if this works, your internet is fine but DNS is the problem
3. Try public DNS: `sudo nano /etc/systemd/resolved.conf` → set `DNS=8.8.8.8 1.1.1.1`
4. Restart: `sudo systemctl restart systemd-resolved`
5. For Docker: check that containers are on the same network

---

### "port not reachable from outside"

**ERROR:** Can access service locally but not from another device or outside your network.

**SOLUTION:**
1. Check firewall: `sudo ufw status` — make sure the port is allowed
2. Check if service is listening on 0.0.0.0 (not just 127.0.0.1): `sudo ss -tlnp | grep <port>`
3. Check router port forwarding (if applicable)
4. Check if you're behind CGNAT (see Chapter 7)
5. Try from a device on mobile data to rule out local network issues

**PH CONTEXT:** Most Philippine ISPs use CGNAT, so port forwarding won't work. Use Tailscale or Cloudflare Tunnel instead (Chapter 7). If you need to confirm CGNAT, compare your router's WAN IP with whatismyipaddress.com.

---

### "WiFi drops intermittently"

**ERROR:** Connection drops every few minutes or under load.

**SOLUTION:**
1. Check WiFi driver: `lspci -k | grep -A 3 -i network`
2. Power saving can cause drops: `iwconfig` → check if power management is on
3. Disable power saving: `sudo iwconfig <interface> power off`
4. **Best fix:** Use Ethernet for your server. WiFi is unreliable for 24/7 services.
5. If you must use WiFi: get a USB 5GHz WiFi adapter (more stable than 2.4GHz)

**PH CONTEXT:** Some Philippine ISPs-provided routers have weak built-in WiFi. If you're using the ISP router as your WiFi access point, consider buying a separate ASUS or TP-Link router.

---

### "DHCP not assigning IP"

**ERROR:** Device gets a `169.254.x.x` address (APIPA) instead of a proper IP.

**CAUSE:** The DHCP server (router) isn't responding.

**SOLUTION:**
1. Renew DHCP lease: `sudo dhclient -r` then `sudo dhclient`
2. Check router DHCP settings — is it enabled? Is the address pool full?
3. Try a static IP temporarily while troubleshooting
4. Restart the router
5. Check for MAC address filtering on the router

**PH CONTEXT:** PLDT and Globe routers sometimes run out of DHCP addresses when many devices connect (phones, laptops, smart TVs, etc.). Restarting the router usually fixes this.

---

### "DNS works but websites won't load"

**ERROR:** `nslookup google.com` works but browsers show "can't connect."

**CAUSE:** DNS is fine, but the actual HTTP/HTTPS connection is failing.

**SOLUTION:**
1. Check if the service is running: `curl http://localhost:PORT`
2. Check firewall: `sudo ufw status`
3. Check if the proxy is working: `curl -v http://example.com`
4. For Docker: check that the container is running and healthy
5. Check MTU settings: `ping -M do -s 1472 google.com` (if this fails, your MTU is too high)

---

### "CGNAT — port forwarding doesn't work"

**ERROR:** Port forwarding rules are configured but external access still fails.

**CAUSE:** Your ISP uses CGNAT (Carrier-Grade NAT), meaning your router doesn't have a public IP.

**SOLUTION:**
1. Confirm CGNAT: Compare your router's WAN IP with whatismyipaddress.com
2. If CGNAT confirmed: use Cloudflare Tunnel or Tailscale (Chapter 7)
3. Call your ISP and ask for a public IP — some will give it for free or for ₱300-₱500/month
4. **PLDT:** Call 171, ask for "public IP para sa homelab"
5. **Globe:** Call 211, ask for "fixed IP" — rarely available
6. **Converge:** Call 8888, sometimes gives public IP on request

---

## Service-Specific Errors

### Vaultwarden: "ADMIN_TOKEN not set"

**ERROR:** Vaultwarden admin panel inaccessible or won't start with admin features.

**SOLUTION:**
1. Set the ADMIN_TOKEN environment variable in your docker-compose.yml:
   ```yaml
   environment:
     - ADMIN_TOKEN=$(openssl rand -base64 48)
   ```
2. Restart: `docker compose restart vaultwarden`
3. Access admin panel at `http://vault.homelab.local/admin`

---

### Vaultwarden: "database locked"

**ERROR:** `database is locked` error in Vaultwarden logs.

**SOLUTION:**
1. Stop Vaultwarden: `docker compose stop vaultwarden`
2. Check for file locks: `lsof /path/to/vaultwarden.db`
3. Ensure only one Vaultwarden container is running
4. Restart: `docker compose up -d vaultwarden`
5. If the database is corrupted: restore from backup

---

### Pi-hole: "FTL not starting"

**ERROR:** Pi-hole container starts but DNS doesn't work. `pihole-FTL` service is down.

**SOLUTION:**
1. Check if port 53 is already in use: `sudo lsof -i :53`
2. If something else is using port 53, stop it or change Pi-hole's DNS port
3. Check Pi-hole logs: `docker logs pihole`
4. Restart: `docker restart pihole`
5. If DNS still doesn't work: set your router's DNS to your Pi-hole IP

**PH CONTEXT:** On some PH routers (especially PLDT), the router's own DNS service may conflict with Pi-hole. Set Pi-hole as the DNS in your router's DHCP settings, not on individual devices.

---

### Pi-hole: "gravity list update failed"

**ERROR:** `pihole -g` fails to download blocklists.

**SOLUTION:**
1. Check DNS resolution: `nslookup gravity.pi-hole.org`
2. If DNS is broken, fix it first (see DNS errors above)
3. Try with a different DNS: `PIHOLE_DNS='8.8.8.8' pihole -g`
4. Check disk space: `df -h`
5. Some blocklists may be temporarily unavailable — try again later

---

### Nextcloud: "Database connection failed"

**ERROR:** Nextcloud shows "Could not connect to database."

**SOLUTION:**
1. Check that the database container is running: `docker ps`
2. Check the database password matches in your `.env` file and docker-compose.yml
3. Check network connectivity: `docker exec nextcloud ping db`
4. Restart the database: `docker compose restart db`
5. If the database is corrupted: restore from backup

---

### Nextcloud: "Memory cache could not be initialized"

**ERROR:** Nextcloud warns about Redis or APCu cache not being configured.

**SOLUTION:**
1. Add Redis to your docker-compose.yml:
   ```yaml
   redis:
     image: redis:alpine
     ports:
       - "127.0.0.1:6379:6379"
   ```
2. Add to Nextcloud config (`config/config.php`):
   ```php
   'memcache.distributed' => '\OC\Memcache\Redis',
   'memcache.local' => '\OC\Memcache\Redis',
   'memcache.locking' => '\OC\Memcache\Redis',
   'redis' => [
     'host' => 'redis',
     'port' => 6379,
   ],
   ```

---

### Jellyfin: "transcoding fails"

**ERROR:** Video won't play or transcoding uses 100% CPU.

**SOLUTION:**
1. Check Jellyfin logs: `docker logs jellyfin`
2. In Jellyfin web UI → Dashboard → Transcoding → check hardware acceleration
3. For Intel Quick Sync (NUC, some mini PCs): enable QSV in settings
4. For ARM (Raspberry Pi): use software transcoding or limit to 720p
5. Check that the GPU device is passed to the container: `--device /dev/dri:/dev/dri`

**PH CONTEXT:** On low-power homelab hardware (N100, i5 8th gen), 1080p transcoding is fine. 4K transcoding requires a newer CPU with Quick Sync or a dedicated GPU. Most PH users are better off converting media to 1080p H.264 once, then streaming the converted version.

---

### Jellyfin: "library scan finds nothing"

**ERROR:** Jellyfin dashboard shows empty library despite having media files.

**SOLUTION:**
1. Check that the media directory is mounted correctly in docker-compose.yml
2. Verify permissions: the Jellyfin container user needs read access to the media files
3. In Jellyfin UI: Settings → Libraries → Scan library again
4. Check Jellyfin logs: `docker logs jellyfin` for folder access errors

---

### Caddy: "auto-HTTPS failed"

**ERROR:** Caddy logs show `auto-HTTPS: no suitable certificate found` or `ACME challenge failed`.

**SOLUTION:**
1. If using a local domain (`.local`, `.home`), Caddy can't get a Let's Encrypt cert — that's expected. Use `http://` not `https://`
2. If using a real domain:
   - Check that DNS points to your server's public IP
   - Check that ports 80 and 443 are open (for Let's Encrypt verification)
   - If behind CGNAT, you can't use Let's Encrypt — use Cloudflare Tunnel instead
   - Check Caddy logs: `docker logs caddy`

---

### Caddy: "reverse proxy: dial: connection refused"

**ERROR:** Caddy starts but services show "502 Bad Gateway."

**SOLUTION:**
1. Check that the target service is running: `docker ps`
2. Check the service is on the same Docker network as Caddy
3. Check the port in your Caddyfile matches the service's actual port
4. If using `network_mode: host` with Caddy: use the server's IP instead of container names
5. Restart Caddy: `docker compose restart caddy`

---

### Prometheus: "TSDB compaction failed"

**ERROR:** Prometheus logs show compaction or WAL errors.

**SOLUTION:**
1. Check disk space: `df -h` (Prometheus stores all metrics — it grows over time)
2. Check Prometheus retention: set `retention.time` in your Prometheus config (e.g., `15d`)
3. Increase storage if needed
4. Restart: `docker compose restart prometheus`

---

### Grafana: "data source is unreachable"

**ERROR:** Grafana dashboard shows "Data source error" for Prometheus or other sources.

**SOLUTION:**
1. Check that Prometheus is running: `docker ps`
2. Check the Grafana data source URL — should be `http://prometheus:9090` (container name, not localhost)
3. Check Docker network — both containers must be on the same network
4. Test connectivity: `docker exec grafana wget -qO- http://prometheus:9090/api/v1/status/config`

---

## The Universal Debugging Checklist

When something doesn't work, go through this list in order. This solves 80% of homelab issues:

1. **Is it running?** `docker ps` — Look for your container
2. **What are the logs?** `docker logs <name>` — Read the error message carefully
3. **Is the port open?** `sudo lsof -i :PORT` — Check port conflicts
4. **Is the firewall blocking it?** `sudo ufw status` — Check allowed ports
5. **Can you reach it locally?** `curl http://localhost:PORT` — Test on the server itself
6. **Can you reach it from another device?** `http://SERVER_IP:PORT` — Test from another machine
7. **Is it a DNS issue?** Try the IP address instead of the hostname
8. **Did you restart after a config change?** `docker compose up -d`
9. **Is it a CGNAT issue?** Check if your ISP uses CGNAT (Chapter 7)
10. **Is your server low on resources?** `htop` and `df -h` — RAM and disk are the usual suspects

---

## When to Ask for Help

Sometimes you need outside help. Here's when and where:

### Good time to ask:
- You've gone through the debugging checklist (above) and still can't figure it out
- The error message is something you've never seen before
- You've tried 3+ solutions from this appendix and nothing works

### Where to ask:
- **Data Engineering Pilipinas (DEP) Discord** — [dataengineering.ph](https://dataengineering.ph/) — Filipino tech community, very helpful
- **r/homelab on Reddit** — Large, active community
- **Docker Hub issues** — For specific container problems
- **Proxmox Forum** — For Proxmox-specific issues
- **GitHub Issues** — For specific software bugs

### How to ask a good question:
1. **What you tried** — List what you've already done
2. **What you expected** — Describe what should have happened
3. **What actually happened** — Include the exact error message
4. **Relevant logs** — `docker logs <container>` or `journalctl -u <service>`
5. **Your setup** — What hardware, what OS, what Docker version

> **🔥 The Chaos Champion:** Intentionally break something. Stop a container, fill up your disk, block a port. Then go through the debugging checklist. Understanding how to fix things when they break is more valuable than knowing how to make them work in the first place.
