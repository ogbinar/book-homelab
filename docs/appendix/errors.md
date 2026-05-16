# Common Errors

_Frequent mistakes and how to fix them._

---

## Docker Errors

### "port is already allocated"

**Symptom:** `docker: Error response from daemon: driver failed programming external connectivity on endpoint ... Bind for 0.0.0.0:8080 failed: port is already allocated.`

**Cause:** Another container or service is already using the port you're trying to map.

**Fix:**
1. Find what's using the port: `sudo lsof -i :8080`
2. Stop that container: `docker stop <container-name>`
3. Or use a different port in your docker run command

**Prevention:** Keep a running list of which ports your services use. The chapter template assigns specific ports to each service — stick to them.

---

### "no space left on device"

**Symptom:** `docker: no space left on device` or `Write failed` errors.

**Cause:** Your disk is full. Docker needs free space for new images and container layers.

**Fix:**
1. Check disk usage: `df -h`
2. Check Docker usage: `docker system df`
3. Clean up: `docker system prune -a --volumes` (careful — removes everything unused)
4. Or remove specific images: `docker image prune -a`

**Prevention:** Run `docker system prune` monthly. Schedule it in cron.

---

### "container exited with non-zero exit code"

**Symptom:** `docker ps` shows a container as `Exited (1)` or `Exited (137)`.

**Cause:** The application inside the container crashed. Exit code 1 = general error. Exit code 137 = killed by OOM (Out of Memory).

**Fix:**
1. Check logs: `docker logs <container-name>`
2. If OOM (exit 137): The container needs more memory. Either give your server more RAM or use a lighter container.
3. If general error: Read the logs. Most container errors explain exactly what went wrong.

**Prevention:** Monitor memory usage. Don't run more containers than your RAM can handle.

---

### "unable to pull image"

**Symptom:** `Error response from daemon: toomanyrequests: You have reached your pull rate limit.`

**Cause:** Docker Hub rate limiting. Free accounts can only pull a certain number of images per 6 hours.

**Fix:**
1. Wait and try again
2. Or log in to Docker Hub: `docker login` (gives you a higher rate limit)
3. Or use a mirror proxy

**Prevention:** Log in to Docker Hub. Cache images locally with `docker pull` before you need them.

---

### "permission denied"

**Symptom:** `Cannot connect to the Docker daemon. Is the docker daemon running?` or `permission denied while trying to connect to the Docker daemon socket`.

**Cause:** Your user isn't in the `docker` group.

**Fix:**
```bash
sudo usermod -aG docker $USER
newgrp docker
```
Then log out and log back in.

**Prevention:** Always add your user to the docker group when setting up Docker.

---

## Network Errors

### "connection refused"

**Symptom:** Browser shows "This site can't be reached" or `curl: (7) Failed to connect`.

**Cause:** The service isn't running, isn't listening on the expected port, or a firewall is blocking the connection.

**Fix:**
1. Check if the container is running: `docker ps`
2. Check if the service is listening: `docker exec <container> netstat -tlnp`
3. Check your firewall: `sudo ufw status`
4. Try accessing from the server itself first: `curl http://localhost:PORT`

**Prevention:** Use auto-restart policies so containers come back automatically.

---

### "DNS resolution failed"

**Symptom:** `Could not resolve hostname` or `ping: unknown host`.

**Cause:** DNS isn't configured correctly. Your computer doesn't know how to translate a hostname to an IP address.

**Fix:**
1. Check `/etc/hosts` for local entries
2. Check your DNS server: `cat /etc/resolv.conf`
3. Try with an IP address instead of a hostname
4. For Docker: containers on the same network can resolve each other by name. Check that they're on the same network.

**Prevention:** Use Docker networks (not `network_mode: host`) when you need container-to-container name resolution.

---

### "can't access from phone"

**Symptom:** Works on your computer but not on your phone.

**Cause:** Your phone might be on a different network (guest WiFi), Pi-hole might be blocking the IP, or the port mapping is wrong.

**Fix:**
1. Make sure your phone is on the same WiFi as your server
2. Try the server's IP directly: `http://192.168.1.100:PORT`
3. Check if Pi-hole is blocking the address
4. Make sure the port is correctly mapped in Docker

**Prevention:** Write down your server's IP and the ports each service uses.

---

## SSH Errors

### "Permission denied (publickey)"

**Symptom:** `ssh: Permission denied (publickey).`

**Cause:** Your SSH key doesn't match what's on the server, or password authentication is disabled.

**Fix:**
1. Make sure you're using the right key: `ssh -i ~/.ssh/your-key username@server`
2. Check the server's authorized keys: `cat ~/.ssh/authorized_keys`
3. If you locked yourself out and have physical access, edit `/etc/ssh/sshd_config` and set `PermitRootLogin yes` and `PasswordAuthentication yes`

**Prevention:** Always test SSH key login BEFORE disabling password authentication.

---

### "Connection timed out"

**Symptom:** `ssh: connect to host ... port 22: Connection timed out.`

**Cause:** Firewall blocking port 22, or you're trying to connect from outside your network without remote access set up.

**Fix:**
1. Check UFW: `sudo ufw status` (port 22 should be allowed)
2. If connecting from outside: Use Tailscale or Cloudflare Tunnel (Chapter 13)
3. Check if your ISP blocks port 22 (some do)

---

## Service-Specific Errors

### Vaultwarden: "ADMIN_TOKEN not set"

**Symptom:** Vaultwarden won't start or admin panel is inaccessible.

**Fix:** Set the `ADMIN_TOKEN` environment variable:
```bash
docker run -e ADMIN_TOKEN=$(openssl rand -base64 48) ...
```

---

### Pi-hole: "FTL not starting"

**Symptom:** Pi-hole container starts but DNS doesn't work.

**Fix:**
1. Check if port 53 is already in use: `sudo lsof -i :53`
2. If something else is using port 53, stop it or change Pi-hole's port
3. Restart: `docker restart pihole`

---

### Nextcloud: "Database connection failed"

**Symptom:** Nextcloud shows "Could not connect to database."

**Fix:**
1. Check that the database container is running: `docker ps`
2. Check the database password in your `.env` file matches the one in the compose file
3. Check network connectivity: `docker exec nextcloud ping db`

---

## The Universal Debugging Checklist

When something doesn't work, go through this list in order:

1. **Is it running?** `docker ps` — Look for your container
2. **What are the logs?** `docker logs <name>` — Read the error message
3. **Is the port open?** `sudo lsof -i :PORT` — Check port conflicts
4. **Is the firewall blocking it?** `sudo ufw status` — Check allowed ports
5. **Can you reach it locally?** `curl http://localhost:PORT` — Test on the server itself
6. **Can you reach it from another device?** `http://SERVER_IP:PORT` — Test from another machine
7. **Is it a DNS issue?** Try the IP address instead of the hostname
8. **Did you restart after a config change?** `docker compose up -d`

---

> **🔥 The Chaos Champion:** Intentionally break something. Stop a container, fill up your disk, block a port. Then go through the debugging checklist. Understanding how to fix things when they break is more valuable than knowing how to make them work in the first place.
