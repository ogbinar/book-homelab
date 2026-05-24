# Chapter 2: Your First Server

## What You'll Build
A working server — your first piece of homelab hardware — with Ubuntu Server installed and accessible via SSH from your main computer. No fancy equipment needed.

## How Long It Takes
30 minutes to 2 hours, depending on your hardware path.

## What You Need
- One of the hardware options below (Path A = ₱0, you already have it)
- A USB flash drive (8GB+, for creating boot media)
- Your main computer (laptop or desktop) for SSH access
- An Ethernet cable (recommended over WiFi for the server)

---

## The Story

Every homelab starts with a server. But "server" sounds intimidating — like something in a cold datacenter with blinking lights and the sound of a jet engine.

Your first server doesn't need to be that.

It could be your old laptop from college. It could be a Raspberry Pi that fits on your desk. It could be a second-hand mini PC you found on Facebook Marketplace for ₱5,000. It could even be a desktop you were going to throw away. *Bakit bawasan pa ang bayad sa cloud kapag may existing na hardware na walang ginagawa?*

**A server is just a computer that stays on and serves something.** That's it. The word makes it sound bigger than it is.

Think of it like this: your laptop IS a server right now. It's serving you. If you installed server software on it and left it running to serve files to your phone, it'd be a file server. The hardware doesn't change — only what it does.

---

## Why This Matters

Your server is the foundation of everything in your homelab. Choose it wisely, but don't overthink it. The hardware doesn't need to be impressive — it needs to be **yours**, and it needs to stay on.

Every homelab starts with a single server. That's it. Everything else — Docker, services, monitoring, backups — builds on top of this one decision: picking a machine and installing an OS.

---

## 🟢 Quick Start

### Path A: ₱0 — Use What You Have

**Your old laptop or desktop.**

Check right now: is there a computer sitting in your house that you don't use anymore? A laptop from 3-5 years ago gathering dust? That's your server.

**Pros:**
- Costs nothing
- Has a built-in battery (UPS included! — the laptop battery keeps it running during brownouts)
- Built-in keyboard and screen for setup
- Quiet and compact

**Cons:**
- Limited upgradeability
- Battery may not hold charge well (old laptops)

### Path B: ₱5,000 — Raspberry Pi 4 or Orange Pi 5

**A single-board computer.**

The Raspberry Pi 4 (4GB or 8GB model) is the most popular entry point into homelabbing worldwide. The Orange Pi 5 is a cheaper alternative with even more powerful hardware.

**Where to buy in the Philippines:**
- **Shopee:** Search "Raspberry Pi 4" — many sellers ship within PH (₱4,500-₱6,000)
- **Lazada:** Official distributors sometimes have sales
- **Facebook Marketplace:** Used Pi 4 boards for ₱3,000-₱4,000
- **Local tech shops:** Computer City (Manila) has Pi accessories

**You'll also need:**
- MicroSD card (32GB, ₱200-₱350)
- USB-C power supply (Pi 4: ₱300-₱500, or reuse an old phone charger if it's 3A)
- Ethernet cable (reuse one from your router)

**Total: ~₱5,000-₱7,000**

### Path C: ₱15,000 — Second-Hand Mini PC

**A used mini PC from Facebook Marketplace or Computer City.**

Look for:
- Intel N3450, N4020, or i5/i7 (6th gen or newer)
- 8GB RAM (upgradeable if possible)
- 128GB+ SSD or eMMC

**Popular models:**
- Intel NUC (various generations)
- Lenovo ThinkCentre Tiny/Micro
- Dell OptiPlex Micro
- HP ProDesk/EliteDesk Mini

**Where to buy:**
- **Facebook Marketplace:** Search "mini PC," "Intel NUC," "used office PC"
- **Computer City (Manila):** Multiple stalls sell refurbished business PCs
- **Shopee/Lazada:** Some sellers specialize in used business PCs

**What to look for when buying used:**
- ✅ Works, turns on, boots to BIOS
- ✅ No cracked screen (if laptop)
- ✅ All USB ports work
- ❌ Walk away if: motherboard damage, strange noises, missing screws (could indicate bad repair)

**⚠️ The Pitfall:** Don't buy the first thing you see. Compare prices across 3-5 listings. ₱15,000 is the target — if someone is asking ₱25,000 for a basic mini PC, keep looking.

### Path D: ₱30,000+ — NUC or Refurbished Server

**For those who want more power from the start.**

A modern Intel NUC (12th gen or newer) with 32GB RAM and 1TB NVMe gives you serious computing power for homelabbing. It's quiet, efficient, and can run 20+ containers comfortably.

A refurbished Dell PowerEdge R730 (₱25,000-₱40,000 on FB Marketplace) gives you enterprise-grade hardware with 64GB+ RAM and 8 drive bays — but it's loud and power-hungry.

---

## 🔵 The Why

Ubuntu Server is the most popular Linux distribution for homelabs. It's free, well-documented, and has the largest community. We're choosing it because:

1. **It's free** — Always
2. **It's stable** — Runs for years without issues
3. **It's well-documented** — Every problem you'll encounter has been solved online
4. **It's what employers use** — Experience transfers directly to jobs

### Step 1: Download Ubuntu Server

Go to [ubuntu.com/server/install](https://ubuntu.com/server/install) and download the latest LTS (Long Term Support) version. LTS versions are supported for 5 years — we want something stable.

**Save the ISO file to your main computer.**

### Step 2: Create a Bootable USB

You need to put the Ubuntu ISO onto a USB flash drive.

**On macOS:**
```bash
# Find your USB drive (replace disk2 with your actual disk)
diskutil list

# Write the ISO (BE VERY CAREFUL — this erases the USB)
sudo dd if=ubuntu-24.04-live-server-amd64.iso of=/dev/rdisk2 bs=1m
```

**On Linux:**
```bash
# Find your USB drive (replace sdb with your actual drive)
lsblk

# Write the ISO
sudo dd if=ubuntu-24.04-live-server-amd64.iso of=/dev/sdb bs=1M status=progress
```

**On Windows:**
Download [Rufus](https://rufus.ie) (free, no installation needed):
1. Open Rufus
2. Select your USB drive
3. Click "SELECT" and choose the Ubuntu ISO
4. Click "START"
5. Confirm the warning (this erases the USB)

> **⚠️ The Pitfall:** When using `dd`, make absolutely sure you're writing to the correct drive. Writing to your main hard drive instead of the USB will erase your computer. Double-check the drive name.

### Step 3: Boot from USB

1. Plug the USB into your server computer
2. Turn it on (or restart if it's already on)
3. Enter the boot menu (usually F12, F9, or Esc during startup — depends on the manufacturer)
4. Select the USB drive

If you're using a Raspberry Pi:
1. Insert the MicroSD card (we'll use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to write Ubuntu)
2. Connect keyboard, monitor, and Ethernet
3. Power it on

### Step 4: Install Ubuntu Server

The Ubuntu Server installer is text-based (no graphics). Don't worry — it's simpler than it looks.

1. **Language:** Select English (or your preference)
2. **Keyboard:** Select your layout (usually "English (US)")
3. **Network:** Connect via Ethernet. The installer should get an IP automatically via DHCP
4. **Proxy:** Leave blank (press Enter) unless your network requires one
5. **Mirror:** Default is fine (Ubuntu servers mirror globally)
6. **Guided storage:** Select "Use an entire disk" → Select your drive → Confirm
   > **📢 Jargon Alert:** "Guided storage" means the installer will automatically partition and format your drive for you. It's the easy option. Advanced users can do manual partitioning, but you're a beginner and that's fine.
7. **SSH setup:** ✅ Check "Install OpenSSH server" — This is CRITICAL. It lets you control your server from your main computer.
8. **Featured servers:** Leave all unchecked — We'll install what we need manually
9. **Ubuntu Pro:** Skip (press Enter, then select "Continue without reporting")
10. **Confirm installation:** Select "Continue"
11. **Wait for installation to complete** — This takes 5-20 minutes depending on your hardware
12. **Remove the USB** when prompted and press Enter to reboot

### Step 5: Your First Login

After reboot, you'll see a terminal prompt. This is your server's command line.

1. Type your username (the one you created during installation)
2. Press Enter
3. Type your password (nothing will appear as you type — this is normal security)
4. Press Enter

You should see a prompt like:
```
username@ubuntu:~$
```

**Welcome to your server. You did it.**

### Step 6: SSH From Your Main Computer

Now the fun part — controlling your server from your main computer without needing a keyboard and monitor attached.

**Find your server's IP address:**
```bash
# On the server itself
ip a
```

Look for a line that says `inet` under `eth0` (Ethernet) or `wlan0` (WiFi). It'll look something like:
```
inet 192.168.1.100
```

That IP address (e.g., `192.168.1.100`) is your server's address on your home network.

**From your main computer (macOS, Linux, or Windows PowerShell):**
```bash
ssh username@192.168.1.100
```

Replace `username` with your actual username and `192.168.1.100` with your server's actual IP.

You'll see:
```
The authenticity of host '192.168.1.100' can't be established.
ED25519 key fingerprint is SHA256:xxxxxxxxxx.
Are you sure you want to continue connecting (yes/no)?
```

Type `yes` and press Enter. Then enter your password.

If it worked, you'll see:
```
Welcome to Ubuntu 24.04 LTS
Last login: [date and time]
username@ubuntu:~$
```

**You are now connected to your server remotely. This is how real sysadmins work.**

> **🚀 Turbo:** Set up SSH keys so you never have to type your password again. See "Go Deeper" below.

---

## Verify Everything Works

Run these commands to check your server:

```bash
# Check system info
uname -a

# Check disk usage
df -h

# Check memory
free -h

# Check running services
systemctl list-units --type=service --state=running

# Update the package list
sudo apt update
```

**Expected output for `df -h`:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G  2.1G   46G   5% /
```
(The size will vary based on your drive)

**Expected output for `free -h`:**
```
              total  used  free  shared  buff/cache  available
Mem:           7.8G  1.2G  4.5G   80M      2.1G       6.3G
Swap:          2.0G    0B  2.0G
```

---

## Stress Test

Now let's prove your server setup works:

1. **Reboot your server** (`sudo reboot`). When it comes back, SSH in. If you can connect, the server rebooted cleanly.
2. **SSH from a different computer.** If you have another device on the same network, try connecting from there. This proves your SSH setup isn't tied to one machine.
3. **Disconnect and reconnect your Ethernet cable.** Watch the SSH session drop, then reconnect. This is what happens during real network issues.
4. **Try `sudo apt update && sudo apt upgrade -y`.** If it runs without errors, your package system is healthy.

> **🔥 The Chaos Champion:** Unplug your server's power cord while you're SSH'd in. Wait 10 seconds. Plug it back in. When it boots, SSH in and run `uptime`. How long was it down? This simulates a real power outage — and it's exactly what happens during Philippine brownouts.

---

## What's Next

You now have a working server that you can access from anywhere on your network. In the next chapter, we'll install Docker — the tool that lets you run applications in isolated, portable packages called containers.

**Homework:**
1. Practice SSH-ing into your server from your main computer
2. Run the verification commands above
3. Update your system: `sudo apt update && sudo apt upgrade -y`
4. Write down your server's IP address somewhere you'll remember it

---

## 🟣 Deep Dive

### Setting Up SSH Keys (Recommended)

SSH keys let you log in without a password. Much more secure and convenient.

**On your main computer:**
```bash
# Generate a key pair (if you don't have one)
ssh-keygen -t ed25519

# Copy the public key to your server
ssh-copy-id username@192.168.1.100
```

**On your server, disable password login for extra security:**
```bash
# Edit the SSH config
sudo nano /etc/ssh/sshd_config

# Find these lines and change them:
# PasswordAuthentication no
# PermitRootLogin no

# Save (Ctrl+O, Enter, Ctrl+X) and restart SSH
sudo systemctl restart ssh
```

> **⚠️ Watch Out:** Before disabling password login, make sure your SSH key works by testing the connection first. If you lock yourself out and don't have physical access to the server, you'll need to connect a keyboard and monitor directly.

---

## 💼 Career Boost

**What you learned that employers care about:**
- Linux system administration (Ubuntu Server)
- Remote server management (SSH)
- Hardware selection and procurement
- System verification and health checks
- Package management (apt)

**Interview talking point:**
> *"I deployed and configured an Ubuntu Server instance accessible via SSH, performing system verification, package management, and security hardening (SSH key authentication). The server runs as the foundation for a multi-service homelab infrastructure."*

---

## 🇵🇭 PH Context

**Power Considerations:**
- Brownouts are real. A laptop (Path A) has a built-in UPS because of its battery.
- For dedicated hardware, consider a basic UPS (₱2,000-₱5,000 on Shopee/Lazada). A 600VA UPS like the CyberPower CP600LCD will give you 15-30 minutes of backup power — enough to save your work and shut down gracefully.
- Meralco average rate (2024): ~₱10.50/kWh. A Raspberry Pi uses ~5W (about ₱175/month). A desktop re purposing might use 50-100W idle (₱1,750-₱3,500/month).

**Internet:**
- Most PH ISPs assign dynamic IPs (they change occasionally). If you need your server accessible from outside your home, see Chapter 7 (Reverse Proxy) for options.
- PLDT, Globe, and Converge all work. Upload speeds are typically the bottleneck (10-50 Mbps upload is common).
- Some ISP plans (PLDT Business, Globe Business) offer static IPs for ~₱1,500-₱3,000/month more.

**Hardware Buying Tips:**
- Facebook Marketplace is your best friend for used hardware. Join groups like "Buy and Sell Used Computers Philippines"
- Computer City (Manila, along Taft Avenue) has dozens of stalls with used business PCs
- Always test before buying used hardware
- Ask sellers for: CPU model, RAM, storage type (SSD vs HDD), and condition

---

> **💸 Lean Path:** Your old laptop is a perfectly valid server. Don't spend money you don't need to spend. Start with ₱0. Upgrade when you hit a real limitation.

> **🇵🇭 PH Context:** A second-hand mini PC from Computer City or FB Marketplace for ₱5,000-₱10,000 is the sweet spot for most beginners. Look for Intel i5 6th gen or newer, 8GB RAM, and an SSD.
