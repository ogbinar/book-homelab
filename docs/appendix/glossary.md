# Glossary

_Terms and definitions used throughout this book._

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

---

## V

**VPN** (Virtual Private Network) — A secure connection between two computers over the internet. Tailscale creates a VPN-like connection between all your devices, so they can talk to each other as if they were on the same network.

---

## W

**Watchtower** — A Docker tool that automatically updates your containers when new images are available. It checks for updates on a schedule and restarts containers with the latest version.

---

## Y

**YAML** — A human-readable data format used for configuration files. Docker Compose files, Kubernetes configs, and GitHub Actions workflows are all written in YAML. It's like JSON but with less punctuation and more indentation.
