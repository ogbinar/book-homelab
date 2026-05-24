# Glossary

_Terms and definitions used throughout this book._

These definitions stay beginner-friendly on purpose. When a term has a Philippines-specific implication, the glossary calls it out instead of assuming you already know the local edge case.

---

## A

**ACME** — Automated Certificate Management Environment. The protocol Cloudflare Tunnel and Caddy use to automatically obtain and renew SSL certificates from certificate authorities like Let's Encrypt.

**Ad blocker** — Software that prevents advertisements from loading on websites and apps. Pi-hole blocks ads at the DNS level, so they never reach your devices.

**API** (Application Programming Interface) — A set of rules that allows different software programs to communicate with each other. Think of it like a restaurant menu: you order what you want, the kitchen prepares it, and you get your food. You don't need to know how the kitchen works.

---

## B

**BIOS/UEFI** — The basic firmware built into your computer that controls what happens when you press the power button. It's the first thing that runs before the operating system loads.

**Bridge (network)** — A Docker network type that creates an isolated network on your machine. Containers on the same bridge network can communicate with each other. This is the default network type.

---

## C

**CGNAT** (Carrier-Grade NAT) — When your ISP doesn't give you a public IP address, you share one with other customers. This means port forwarding doesn't work. Most Philippine ISPs use CGNAT by default.

**CIDR** (Classless Inter-Domain Routing) — A notation for IP addresses that tells you how many devices are on a network. `/24` means 254 possible devices (192.168.1.1 through 192.168.1.254).

**Container** — A lightweight, standalone package that includes everything needed to run a piece of software (code, libraries, settings). Docker containers are like shipping containers: standardized, portable, and easy to move between servers.

**Cron** — A time-based job scheduler in Linux. You tell it "run this command at 3 AM every day" and it does it automatically.

---

## D

**Default Gateway** — The device (usually your router) that routes traffic between your local network and the internet. Think of it as the doorway between your home and the outside world.

**DHCP** (Dynamic Host Configuration Protocol) — The system that automatically assigns IP addresses to devices on your network. Your router's DHCP server says "You can be 192.168.1.105. Use it."

**Docker** — A platform that lets you run applications in containers. Instead of installing software directly on your server, you run it in a Docker container that's isolated from the rest of the system.

**Docker Compose** — A tool that lets you define and run multi-container Docker applications in a single YAML file. Instead of running 10 separate `docker run` commands, you write one `docker-compose.yml` and run `docker compose up`.

**DNS** (Domain Name System) — The phonebook of the internet. It translates human-readable domain names (like `google.com`) into IP addresses (like `142.250.80.46`) that computers use to communicate.

**Docker Volume** — A way to store data outside of containers so it persists when containers are deleted. Think of it like a USB drive for your container's data.

---

## F

**Fail2ban** — A security tool that monitors log files and temporarily bans IP addresses that show malicious behavior (too many failed login attempts, etc.).

**Firewall** — A network security system that filters incoming and outgoing traffic based on rules. UFW (Uncomplicated Firewall) is Ubuntu's built-in firewall. Think of it as a bouncer at a club: it decides who gets in and who stays out.

---

## H

**HTTP** (HyperText Transfer Protocol) — The protocol used for loading web pages. When you see `http://` in your browser, that's HTTP. It sends data in plain text — anyone on the network can read it.

**HTTPS** (HyperText Transfer Protocol Secure) — HTTP with encryption. The `S` stands for Secure. Data is encrypted between your browser and the server, so eavesdroppers can't read it.

---

## I

**Idempotent** — Running this command 10 times has the same result as running it once. Like pressing the "play" button on Spotify — pressing it once or ten times, you get the same song playing.

**Image** — A Docker template that contains the software, libraries, and settings needed to run a container. Think of it like a recipe: the image is the recipe, the container is the cake.

**Infrastructure as Code (IaC)** — Managing computing infrastructure through machine-readable definition files, instead of manual configuration. Docker Compose is your first step into IaC.

---

## M

**MTBF** (Mean Time Between Failures) — A measure of how long a system runs before something breaks. In homelabbing, your MTBF is measured in days, not years.

**NAT** (Network Address Translation) — The process by which your router allows multiple devices on your home network to share a single public IP address. When your phone requests google.com, NAT replaces your private IP with your public IP and tracks which request belongs to which device.

---

## O

**Observability** — The ability to understand the internal state of a system by examining its outputs (metrics, logs, traces). Monitoring is collecting the data. Observability is understanding what it means.

---

## P

**Port** — A numbered endpoint for network communication. Think of it like a door number in a building. The IP address is the building; the port is which room you want to enter. Port 80 is HTTP, port 443 is HTTPS, port 22 is SSH.

**Private IP** — An IP address that only works inside your home network (192.168.x.x, 10.x.x.x). Devices with private IPs can't be reached from the internet directly.

**Proxmox** — A virtualization platform that lets you run multiple virtual machines. Not covered in this book (we use Docker), but worth knowing about if you want to run full operating systems, not just containers.

---

## R

**Reverse Proxy** — A server that sits in front of other servers and forwards client requests to the appropriate backend server. A regular proxy forwards on behalf of the client; a reverse proxy forwards on behalf of the server. Think of it as a receptionist: you talk to the receptionist, and they route you to the right department.

**RPO** (Recovery Point Objective) — How much data can you afford to lose? If you backup daily, your RPO is 24 hours.

**RTO** (Recovery Time Objective) — How long does it take to recover? If you can rebuild your homelab in 30 minutes, your RTO is 30 minutes.

---

## S

**SSH** (Secure Shell) — A protocol for securely connecting to a remote computer. Instead of typing commands on your server's keyboard, you connect from your laptop using `ssh username@server-ip`. Everything is encrypted.

**SSL/TLS** — Protocols for encrypting communications between computers. When you see the lock icon 🔒 in your browser, that's TLS. It encrypts all data between your browser and the website.

---

## U

**UFW** (Uncomplicated Firewall) — Ubuntu's simplified firewall tool. It makes it easy to allow or block traffic on specific ports.

---

## V

**Volume** — See "Docker Volume" above.

**VPN** (Virtual Private Network) — A secure connection between two computers over the internet. Tailscale creates a VPN-like connection between all your devices, so they can talk to each other as if they were on the same network.

---

## W

**Watchtower** — A Docker tool that automatically updates your containers when new images are available. It checks for updates on a schedule and restarts containers with the latest version.

---

## Y

**YAML** — A human-readable data format used for configuration files. Docker Compose files, Kubernetes configs, and GitHub Actions workflows are all written in YAML. It's like JSON but with less punctuation and more indentation.

---

## E (added)

**Encryption** — The process of converting data into a code to prevent unauthorized access. When data is encrypted, only someone with the correct "key" can read it. HTTPS, SSH, and VPNs all use encryption.

**Error 502/503/504** — Common HTTP error codes from your reverse proxy. 502 = "Bad Gateway" (the backend service is down or unreachable). 503 = "Service Unavailable" (the server is too busy). 504 = "Gateway Timeout" (the backend took too long to respond).

---

## L (added)

**Latency** — The delay between sending a request and receiving a response. Measured in milliseconds (ms). Low latency = fast response. Your homelab on LAN should have latency under 1ms. Internet latency depends on your ISP.

**Load Balancer** — A tool that distributes network traffic across multiple servers. In a homelab, this is rarely needed (you probably have one server). But it's a concept you'll encounter in professional infrastructure.

**Localhost / 127.0.0.1** — Refers to your own computer. When a service binds to localhost, it can only be accessed from the same machine. Useful for services that don't need external access.

---

## T (added)

**Tailscale** — A tool that creates a secure, encrypted mesh VPN between your devices. Install it on your server and phone, and you can access your homelab from anywhere. Works through CGNAT without port forwarding. Free for personal use.

**Threadfin** — A TV guide (EPG) server for Jellyfin and other media players. Provides program listings for recorded or streamed TV.

**Transcoding** — Converting a video from one format to another in real-time. Needed when your playback device can't natively play the video format. Hardware transcoding (using GPU) is faster and uses less CPU.

**UUID** — Universally Unique Identifier. A 128-bit number used to uniquely identify devices, files, or volumes. In Linux, `blkid` shows UUIDs for storage devices, which are used in `/etc/fstab` for persistent mounting.

---

## P (added)

**Packet** — A small unit of data sent over a network. When you load a webpage, it's broken into thousands of packets, each traveling independently, then reassembled at the destination. Think of it like sending a letter by tearing it into pieces, mailing each piece separately, and reassembling at the other end.

**Ping** — A simple network tool that checks if a device is reachable. Sends a small packet and waits for a response. `ping 8.8.8.8` checks if your internet connection works. Response time is measured in milliseconds.

**Port Forwarding** — A router configuration that directs incoming internet traffic on a specific port to a device on your local network. For example, forwarding port 443 to your reverse proxy server. Requires a public IP address (doesn't work with CGNAT).

**Proxy** — An intermediary server that forwards requests between a client and a server. A *forward* proxy sits between clients and the internet (used by corporations to filter traffic). A *reverse* proxy sits in front of servers (used in homelabs to route traffic).

**Public IP** — The unique address your router presents to the internet. Assigned by your ISP. If you want to access your homelab from outside your home (from a coffee shop, for example), you need to understand public IPs and port forwarding. Most PH ISPs use CGNAT, so you don't get a true public IP.

---

## S (added)

**Snapshot** — A point-in-time copy of a virtual machine or storage volume. If something goes wrong, you can restore to the last known good snapshot. Not the same as a backup — snapshots are tied to the same storage system.

**Subnet** — A smaller network within a larger network. Your home network `192.168.1.0/24` is a subnet — it can hold up to 254 devices. A `/24` subnet means the first three parts of the IP (192.168.1) are shared, and the last part identifies individual devices.

**Swap** — A portion of disk space used as "extra RAM" when your physical memory is full. When RAM is full, the system moves inactive data to swap. Swap is much slower than RAM but prevents crashes. On a Raspberry Pi, excessive swap can wear out the microSD card.

**Sysadmin** — Short for "Systems Administrator." The person (or role) responsible for keeping servers running, secure, and performing well. Homelabbing is essentially sysadmin practice — and it's one of the best ways to build sysadmin skills.

**Systemd** — The init system and service manager used by Ubuntu and most modern Linux distributions. It controls what services start at boot, monitors running services, and manages system state. Commands like `systemctl restart ssh` or `systemctl enable docker` use systemd.

---

## N (added)

**Node** — A single machine (physical or virtual) in a network. In Kubernetes, a "node" is a worker machine that runs containers. In Prometheus, "node-exporter" collects metrics from a single server.

**Nomad** — A simple, flexible orchestrator for containers and non-containerized applications. Created by HashiCorp (the same team behind Vagrant, Consul, and Terraform). Less complex than Kubernetes — a good alternative for homelabs.

---

## G (added)

**Grep** — A command-line tool for searching text. `grep "error" /var/log/syslog` finds all lines containing "error" in the syslog file. Essential for log analysis and troubleshooting.

**GUI** — Graphical User Interface. The visual way of interacting with a computer (windows, icons, mouse). Ubuntu Server has no GUI — you interact with it entirely through the command line (CLI). This is intentional: servers don't need GUIs, and removing them saves resources.

---

## R (added)

**Rate Limiting** — Restricting how many requests a client can make in a given time period. Protects against DDoS attacks and abuse. Cloudflare Tunnel has built-in rate limiting. You can also configure it in Caddy or Traefik.

**Recovery** — The process of restoring systems and data after a failure. Includes restoring from backups, rebuilding servers, and verifying that everything works. Your RTO (Recovery Time Objective) measures how fast you can recover.

---

## M (added)

**Markdown** — A lightweight markup language for formatting text. Uses simple syntax like `**bold**`, `# headings`, and `- lists`. GitHub README files, this book, and most documentation are written in Markdown. Easy to learn, powerful for documentation.

**Memory (RAM)** — Random Access Memory. Temporary, fast storage that your server uses for running applications. Unlike disk storage, RAM is volatile — data is lost when power is cut. Docker containers, databases, and services all run in RAM. The more RAM, the more services you can run simultaneously.

**Metadata** — Data about data. For example, a photo's EXIF data (date taken, camera model, GPS coordinates) is metadata about the photo. In Docker, labels are metadata attached to containers. In Grafana, metric labels describe what the data represents.

**Middleware** — Software that sits between an operating system and applications, facilitating communication between them. A reverse proxy is a type of middleware. In the homelab context, Caddy acts as middleware between clients and your services.

---

## C (added)

**Chroot** — A command that changes the apparent root directory for a running process and its children. Used for isolating processes and creating controlled environments. Pi-hole uses chroot for its DNS resolver.

**Commit (Git)** — A saved snapshot of your repository's changes. Each commit has a message describing what changed. Think of it like saving a game — you can always go back to a previous save point.

**Container Registry** — A storage and distribution service for container images. Docker Hub is the default and largest registry. GitHub Container Registry (GHCR) and GitLab Container Registry are alternatives. You "pull" images from registries and "push" your own images to them.

**Cron Job** — A scheduled task in Linux. Defined in the crontab file, cron jobs run automatically at specified times. Example: `0 3 * * * /path/to/backup.sh` runs a backup script every day at 3 AM.

**Custom Domain** — Your own domain name (like `myhomelab.com`) instead of a provider's subdomain. Gives you full control over DNS records and SSL certificates. Costs ~₱500-₱1,500/year for a .com domain.

---

## D (added)

**Daemon** — A background process that runs continuously. In Linux, daemon names often end in "d" (sshd, docker, cron). Docker containers run daemons inside them. If a daemon crashes, the associated service goes down.

**Debug** — The process of finding and fixing errors in software or configuration. `docker logs <container>` is one of the most common debugging tools. Reading error messages carefully is the first step in debugging.

**Docker Hub** — The default container registry for Docker. Hosts millions of container images, from official images (like `ubuntu:24.04`) to community-maintained images (like `louislam/uptime-kuma:1`). Think of it as an "app store" for containers.

**Docker Stats** — A command (`docker stats`) that shows real-time resource usage for running containers: CPU%, memory usage, network I/O, and block I/O. Essential for monitoring which containers are resource-hungry.

**Domain** — A human-readable name for an IP address. Instead of remembering `142.250.80.46`, you type `google.com`. Domains are managed through DNS, and you register them through domain registrars like Namecheap, Cloudflare, or DotPH.

**Downtime** — Period when a service is unavailable. Measured in uptime percentage: 99.9% uptime = ~8.76 hours of downtime per year. Your homelab won't achieve 99.9% — and that's okay. The goal is progressive reliability.

---

## K (added)

**kubectl** — The command-line tool for interacting with Kubernetes clusters. You use it to deploy applications, inspect and manage cluster resources, and view logs. Even if you don't use Kubernetes now, knowing `kubectl` is valuable for career growth.

**Kubernetes (K8s)** — An open-source container orchestration platform that automates deploying, scaling, and managing containerized applications. More complex than Docker Compose but essential for large-scale deployments. Worth learning after you're comfortable with Docker Compose.

---

## O (added)

**Open Source** — Software whose source code is freely available for anyone to view, modify, and distribute. Docker, Ubuntu, Caddy, Pi-hole, and Grafana are all open source. Open source is the foundation of the homelab movement — no vendor lock-in, no hidden costs.

**Outage** — A period when a service or network is completely unavailable. Different from downtime in that outages are usually unexpected and affect multiple users. PH internet outages (from ISP issues, typhoons, or fiber cuts) are common reasons for homelab outages.

---

## A (added)

**Affinity (Docker)** — A Docker scheduling concept that tells containers to run on specific nodes or avoid certain nodes. In a single-server homelab, this isn't relevant. But in multi-server setups with Kubernetes or Swarm, affinity rules determine where containers are placed.

**Anacron** — A job scheduler that handles tasks that should run periodically but may not run at the specified time (e.g., when the computer is off). Useful for laptops and systems that aren't always running. Alternative to cron for non-24/7 systems.

---

## B (added)

**Bandwidth** — The maximum amount of data that can be transmitted over your internet connection in a given time, usually measured in Mbps (megabits per second). Your download speed determines how fast you can pull Docker images. Your upload speed determines how fast others can access your homelab remotely.

**Bash** — Bourne Again Shell. The default command-line interpreter (shell) for Ubuntu and most Linux distributions. When you open a terminal, you're running bash. Bash scripts let you automate sequences of commands.

**Boot** — The process of starting a computer and loading the operating system. Different from "resume" (waking from sleep). Your server should be configured to boot automatically after a power outage — check BIOS/UEFI settings for "Restore on AC Power Loss."

**BorgBackup** — A deduplicating backup program that supports compression, encryption, and authenticated encryption. Designed for efficiency and security. More advanced than simple rsync backups, suitable for large datasets with excellent deduplication.

---

## F (added)

**Fstab** — The filesystem table (`/etc/fstab`) in Linux defines how disk partitions and other storage devices are mounted. Each line specifies a device (by UUID or path), mount point, filesystem type, and options. Critical for ensuring your backup drive mounts automatically at boot.

**Full-Text Search** — The ability to search through all content in a database or document store. Nextcloud, Jellyfin, and other services support full-text search, letting you find files, metadata, or content quickly.

**Flood** — A common natural disaster in the Philippines that can damage electronic equipment. If you live in a flood-prone area, keep your homelab hardware on high shelves or elevated platforms. Consider waterproof containers for external drives.

---

## H (added)

**Hostname** — The name given to a device on a network. Your server's hostname might be `ubuntu` or `homelab-server`. Set it with `sudo hostnamectl set-hostname homelab`. Hostnames are more memorable than IP addresses and are used in DNS resolution.

**HTTP Status Codes** — Three-digit numbers returned by web servers to indicate the result of a request. 200 = Success, 301 = Redirect, 403 = Forbidden, 404 = Not Found, 500 = Server Error, 502 = Bad Gateway, 503 = Service Unavailable. You'll see these in your reverse proxy logs.

---

## I (added)

**Incident** — An unplanned interruption or reduction in quality of service. In professional infrastructure, incidents are tracked, investigated, and resolved. Your first "incident" might be a service crashing after a Docker update. Documenting incidents helps you prevent them from happening again.

**Ingress (Kubernetes)** — A Kubernetes resource that manages external access to services in a cluster, typically HTTP. Works similarly to a reverse proxy. If you ever move from Docker Compose to Kubernetes, Ingress is your equivalent of Caddy/Traefik configuration.

---

## J (added)

**Jellyfin** — A free, open-source media server that lets you stream your personal media collection (movies, music, photos) to any device. Alternative to Plex. Runs in a Docker container, supports direct playback and hardware transcoding.

**JSON** — JavaScript Object Notation. A lightweight data-interchange format used in APIs, configuration files, and data exchange. Similar to YAML but more punctuation-heavy. Docker's `inspect` command outputs JSON.

---

## P (added - more)

**PHP** — Hypertext Preprocessor. A server-side scripting language used by many web applications including Nextcloud, WordPress, and phpMyAdmin. Nextcloud runs on PHP, so you'll encounter it if you self-host Nextcloud. Docker images handle PHP automatically.

**PHP-FPM** — PHP FastCGI Process Manager. The process manager that handles PHP requests for web servers like Apache and Nginx. Nextcloud uses PHP-FPM for performance. You don't need to configure this directly — the Nextcloud Docker image handles it.

**Podman** — A daemonless container engine that is an alternative to Docker. Compatible with Docker commands (`podman` works like `docker`). Gaining popularity as a Docker alternative, especially in enterprise environments. Uses rootless containers by default.

---

## Q (added)

**QEMU** — Quick Emulator. A free and open-source emulator and virtualizer that can run operating systems and programs made for one machine on a different machine. Used by Docker Desktop on macOS and Windows. On Linux, Docker uses containers directly without QEMU.

---

## R (added - more)

**RAID** — Redundant Array of Independent Disks. A data storage virtualization technology that combines multiple disk components into a single logical unit. RAID 1 (mirroring) provides redundancy — if one drive fails, data is still available. RAID 5/6 provide redundancy with better storage efficiency. Not covered in this book (requires multiple drives), but worth knowing.

**Read-Only** — A file or volume permission that prevents writing. Mounting a volume as read-only (`:ro` in Docker) is a security best practice for configuration files that shouldn't be modified by containers.

**Regex** — Regular Expression. A sequence of characters that defines a search pattern. Used in grep, sed, and many configuration files. Example: `grep -E "error|fail|warning" /var/log/syslog` finds lines containing any of those three words.

**Remote Desktop** — A protocol for controlling a computer from a remote location. VNC, RDP, and NoMachine are common protocols. Useful for managing servers without command-line access, but uses more bandwidth than SSH.

**REST API** — Representational State Transfer API. A web API architecture that uses HTTP requests to access data. Most modern services (including Vaultwarden, Nextcloud, and Uptime Kuma) expose REST APIs that you can use for automation.

---

## T (added - more)

**TCP** — Transmission Control Protocol. One of the main protocols of the Internet Protocol suite. TCP ensures that data packets arrive in order and without errors. HTTP, SSH, and most web traffic use TCP. Reliable but slower than UDP.

**TLS** — Transport Layer Security. The successor to SSL. TLS encrypts data in transit between two systems. When you see the lock icon in your browser, that's TLS. Caddy and Cloudflare Tunnel both use TLS to encrypt your traffic.

**Troubleshooting** — The process of diagnosing and resolving problems. In homelabbing, troubleshooting involves: reading error messages, checking logs, testing connectivity, and isolating variables. The most common commands: `docker logs`, `curl`, `ping`, `systemctl status`.

**Tunnel** — A method of encapsulating network traffic to create a secure connection between two points over an untrusted network. Cloudflare Tunnel and Tailscale both create tunnels. Tunnels bypass CGNAT because they initiate an *outgoing* connection (not incoming).

---

## U (added - more)

**Ubuntu** — A Linux distribution based on Debian. Ubuntu Server is the version used in this book. LTS (Long Term Support) versions are supported for 5 years and are recommended for servers. Ubuntu 24.04 LTS is the latest LTS as of this writing.

**Uptime** — The amount of time a system is operational and available. Measured as a percentage: 99% uptime = ~87.6 hours downtime per year. Your homelab's uptime depends on power stability, hardware reliability, and your maintenance practices.

**Upstream** — The original source of software or data. In Docker, the "upstream" image is the original image on Docker Hub. In Git, the "upstream" remote is the original repository you forked from. In networking, "upstream" refers to the connection toward the internet.

---

## X (added)

**XFS** — A high-performance filesystem used by many Linux distributions. Excellent for large files and parallel I/O operations. Common choice for storage drives in homelab setups. Alternative filesystems include ext4 (more universal) and Btrfs (advanced features like snapshots).
