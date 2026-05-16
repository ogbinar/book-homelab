# Chapter 12: Security — Don't Get Hacked

## What You'll Build
A hardened homelab with firewall rules, SSH key authentication, fail2ban, and security best practices. You'll understand the most common homelab vulnerabilities and how to prevent them.

## How Long It Takes
2 hours (including reading and configuration)

## What You Need
- Your homelab stack running (Chapters 1-11)
- Root or sudo access to your server

---

## The Story

Here's what happens when you expose anything to the internet: **bots find it.** Within minutes of connecting to the internet, your server is being scanned by automated bots looking for:

- Open SSH ports with password login
- Unprotected web interfaces
- Outdated software with known vulnerabilities
- Default passwords on services

> ⚠️ **The Pitfall:** Your homelab is not "just a hobby." To a bot, it's just another server with an IP address. Bots don't care that you're learning — they'll try to exploit your server just like they'd try to exploit a production server.

This chapter isn't about fear. It's about **knowing what you're doing.** Every step here has a reason. Skip them at your own risk.

---

## Why This Matters

A compromised homelab isn't just embarrassing. It can be:
- **Used as a botnet node** (your server participates in DDoS attacks)
- **Used for crypto mining** (your electricity bill goes up, hardware degrades)
- **Used to attack other devices** on your network
- **Used to steal your data** (passwords, documents, photos)

Securing your homelab protects:
1. **Your data** — from theft and encryption
2. **Your network** — from lateral attacks
3. **Your reputation** — from being part of a botnet
4. **Your wallet** — from crypto mining and electricity costs

---

## 🟢 Quick Start

### Step 1: SSH Key Authentication (No More Passwords)

Password-based SSH is the #1 way servers get compromised. Keys are essentially unbreakable.

**Generate a key pair on your main computer:**
```bash
ssh-keygen -t ed25519 -C "homelab@main-computer"
```

**Copy the key to your server:**
```bash
ssh-copy-id username@192.168.1.100
```

**Test it:**
```bash
ssh username@192.168.1.100
```

You should log in WITHOUT a password prompt.

**Disable password authentication on the server:**
```bash
sudo nano /etc/ssh/sshd_config
```

Find and change these lines:
```
PasswordAuthentication no
PermitRootLogin no
```

Restart SSH:
```bash
sudo systemctl restart ssh
```

> **⚠️ THE MOST IMPORTANT WARNING:** Before disabling password authentication, MAKE SURE your SSH key works. Test the connection from your main computer. If you lock yourself out and don't have physical access to the server (or IPMI/KVM), you'll need to connect a keyboard and monitor directly. **DO NOT SKIP THIS TEST.**

### Step 2: Configure the Firewall (UFW)

Ubuntu comes with UFW (Uncomplicated Firewall). Let's set it up:

```bash
# Install UFW (if not already installed)
sudo apt install ufw

# Reset to clean state
sudo ufw reset

# Default policies: deny all incoming, allow all outgoing
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH (CRITICAL — don't lock yourself out!)
sudo ufw allow 22/tcp

# Allow HTTP/HTTPS (for your reverse proxy)
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Allow ONLY the ports you actually need
# Example: If you use Uptime Kuma on 3001 locally
# sudo ufw allow 3001/tcp

# Enable the firewall
sudo ufw enable

# Check status
sudo ufw status numbered
```

**Expected output:**
```
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22/tcp                     ALLOW IN    Anywhere
[ 2] 80/tcp                     ALLOW IN    Anywhere
[ 3] 443/tcp                    ALLOW IN    Anywhere
```

> **📢 Jargon Alert:** "Firewall" — A network security system that filters incoming and outgoing traffic based on rules. Think of it as a bouncer at a club: it decides who gets in and who stays out. By default, we deny everything and only allow what we need.

### Step 3: Install Fail2ban

Fail2ban monitors log files and bans IPs that show malicious behavior (too many failed login attempts, etc.).

```bash
sudo apt install fail2ban

# Create custom config (overrides the default)
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

```ini
# jail.local — Fail2ban configuration

[DEFAULT]
# Ban anyone who fails 5 times within 10 minutes
bantime  = 3600      # 1 hour ban
findtime = 600       # 10 minute window
maxretry = 5

# Use email notifications (optional)
# action = %(action_mw)s

[sshd]
enabled  = true
port     = ssh
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 3
bantime  = 7200      # 2 hours for SSH
```

Restart fail2ban:
```bash
sudo systemctl restart fail2ban
sudo systemctl enable fail2ban
```

**Check active bans:**
```bash
sudo fail2ban-client status sshd
```

### Step 4: Secure Your Docker Containers

Docker containers can sometimes expose services unintentionally. Here's how to lock them down:

**Bind containers to localhost only (for services that don't need external access):**

In your `docker-compose.yml`, change port mappings:

```yaml
# Instead of this (exposed to entire network):
ports:
  - "3001:3001"

# Use this (only accessible from the server itself):
ports:
  - "127.0.0.1:3001:3001"
```

**Remove unnecessary ports:**
```bash
# Check what ports are exposed
docker ps --format "table {{.Names}}\t{{.Ports}}"

# Stop and remove any containers you don't need
docker stop <unnecessary-container>
docker rm <unnecessary-container>
```

**Update containers regularly:**
```bash
# This should be automated (Chapter 10), but verify:
docker compose pull
docker compose up -d
```

### Step 5: Service-Level Security

**Nextcloud:**
```bash
# Enable HTTPS (if using a reverse proxy, this is handled automatically)
# Enable Two-Factor Authentication for all users
# Disable public sharing by default
```

**Vaultwarden:**
```bash
# Set ADMIN_TOKEN (already done in docker-compose.yml)
# Disable signups: -e SIGNUPS_ALLOWED=false
# Enable 2FA enforcement: -e ENABLE_2FA=true
```

**Pi-hole:**
```bash
# Set a strong admin password (already done in docker-compose.yml)
# Enable DNSSEC: Add to /etc/pihole/setupVars.conf:
# DNSSEC=true
```

---

## 🔵 The Why

### Security Layers (Defense in Depth)

```
Internet
  │
  ▼
┌─────────────────────┐
│   Fail2ban          │  ← Blocks brute-force attackers
│   (at SSH gateway)  │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│   UFW Firewall      │  ← Only allows needed ports
│   (network level)   │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│   SSH Keys          │  ← No password attacks possible
│   (authentication)  │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│   Docker Security   │  ← Isolated containers, minimal exposure
│   (application)     │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│   Backup            │  ← Last line of defense
│   (recovery)        │
└─────────────────────┘
```

This is **defense in depth** — multiple layers of security so that if one fails, others still protect you.

### Why Each Step Matters

| Step | What It Prevents | Real-World Example |
|---|---|---|
| SSH keys | Brute-force password attacks | 90% of SSH attacks use automated password guessing |
| UFW firewall | Unauthorized port access | Bots scan all 65,535 ports looking for open services |
| Fail2ban | Repeated login attempts | A single server sees 10,000+ failed SSH attempts/day |
| Docker binding | Lateral movement | A compromised container can't reach other services |
| Regular updates | Known vulnerabilities | 60% of breaches exploit known, patchable vulnerabilities |

### When NOT to Expose Services

**Never expose these to the internet without proper security:**
- Databases (PostgreSQL, MySQL, MongoDB) — NEVER
- Administration panels — Only via VPN or reverse proxy with auth
- Services without authentication — NEVER
- Services with default passwords — NEVER

**Safe to expose (with reverse proxy + SSL):**
- Nextcloud (has its own auth)
- Jellyfin (has its own auth)
- Uptime Kuma (has its own auth)
- Pi-hole admin (password-protected)

> **🧠 The Deep Dive:** The most secure service is the one you don't expose. If a service only needs to be accessed on your local network, don't open any ports for it. Use your reverse proxy for local access only.

---

## 🟣 Deep Dive

### Security Audit Checklist

Run this monthly:

```bash
# 1. Check for open ports
sudo ufw status

# 2. Check SSH configuration
grep -E "PasswordAuth|PermitRoot" /etc/ssh/sshd_config

# 3. Check for failed SSH attempts
sudo grep "Failed password" /var/log/auth.log | wc -l

# 4. Check active fail2ban bans
sudo fail2ban-client status

# 5. Check for outdated packages
apt list --upgradable

# 6. Check running containers and exposed ports
docker ps --format "table {{.Names}}\t{{.Ports}}"

# 7. Check disk usage
df -h
```

### Network Segmentation

For advanced security, create separate Docker networks:

```yaml
networks:
  public:     # Services accessible from the internet (via reverse proxy)
  internal:   # Services only accessible internally
  database:   # Database services only
    driver: bridge
    internal: true
```

Then assign services to appropriate networks based on their exposure requirements.

---

## 💼 Career Boost

**What you learned that employers care about:**
- Firewall configuration (UFW)
- SSH hardening
- Intrusion prevention (fail2ban)
- Container security
- Security audit methodology
- Defense in depth principles

**Interview talking point:**
> *"I implemented a defense-in-depth security strategy for my homelab infrastructure: UFW firewall with least-privilege port rules, SSH key-only authentication with root login disabled, fail2ban for intrusion prevention, Docker network segmentation, and monthly security audit procedures. This reduced the attack surface by 90% and achieved zero unauthorized access attempts in 6 months."*

---

## 🇵🇭 PH Context

### Philippine-Specific Threats

| Threat | Likelihood | Mitigation |
|---|---|---|
| Brute-force SSH attacks | Very High | SSH keys + fail2ban |
| Web service scanning | High | Reverse proxy + SSL |
| Router exploit attempts | Medium | Keep router firmware updated |
| Physical theft | Variable | Encrypt sensitive data |

### Data Privacy Act of 2012

If your homelab stores personal data (which Nextcloud, Vaultwarden, or any service might), you should be aware of the **Philippine Data Privacy Act (RA 10173)**:

- You're responsible for protecting personal data you store
- Breaches must be reported to the National Privacy Commission
- Reasonable security measures are required

Your homelab security practices (encryption, access controls, backups) help you comply with this law.

---

## Stress Test

Now let's prove your security works:

1. **Try to SSH with a wrong password** from a different device. Watch fail2ban in action: `sudo fail2ban-client status sshd`. After 3 failed attempts, the IP should be banned. This proves fail2ban is working.
2. **Try to access a closed port** — use `nmap -p 22,80,443,3001,8080 YOUR_SERVER_IP` from another device on your network. Only the ports you allowed through UFW should show as open. Everything else should be filtered. This proves your firewall is working.
3. **Re-enable password authentication** (`PasswordAuthentication yes`), then disable it again. Restart SSH. Test that password login is rejected but key login still works. This proves you can recover from mistakes.
4. **Run the full security audit** (the checklist from the Deep Dive section). Document the results. This becomes your security baseline — compare future audits against it.

> **🔥 The Chaos Champion:** Disable password authentication WITHOUT testing your SSH key first. Then try to SSH in from a device that doesn't have your key. You'll be locked out. If you have physical access to the server, fix it. If you don't, you've learned the most important lesson in homelab security: **always test before you commit.** This is the mistake that catches everyone — even experienced admins. Learn from it.

---

## What's Next

Your homelab is now secure. You've gone from a curious beginner to someone who can build, secure, monitor, and maintain a production-grade infrastructure at home. In Chapter 14, we'll document everything you've built and turn it into a portfolio that shows employers: **here's what I can do.**

**Homework:**
1. Set up SSH key authentication and disable password login
2. Configure UFW firewall with only necessary ports
3. Install and configure fail2ban
4. Run the security audit checklist
5. Document your security configuration

---

## Go Deeper

- [UFW Documentation](https://help.ubuntu.com/community/UFW) — Ubuntu firewall guide
- [Fail2ban Documentation](https://fail2ban.readthedocs.io/) — Intrusion prevention
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) — Web application security risks
- [CIS Benchmarks](https://www.cisecurity.org/benchmark/ubuntu_linux) — Security configuration standards
- [Philippine Data Privacy Act (RA 10173)](https://pcpc.gov.ph/laws-and-regulations/data-privacy/) — PH data protection law
