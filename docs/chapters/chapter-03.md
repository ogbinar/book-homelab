# Chapter 3: Hello, Container

## What You'll Build
Docker installed and running on your server, with your first container up and running. You'll understand what containers are, why they matter, and how to run your first real service.

## How Long It Takes
1 hour (including reading and hands-on steps)

## What You Need
- Your server from Chapter 2 (running Ubuntu Server, accessible via SSH)
- Internet connection on the server

---

## The Story

Imagine you're moving to a new apartment. You have books, kitchenware, clothes, electronics — all different shapes and sizes. How do you pack them efficiently?

One way: wrap each item individually, figure out custom boxes for everything, hope nothing breaks. That's how people used to install software on servers.

The other way: use standard shipping containers. A container is the same size, same corners, same structure — regardless of whether it's full of phones, clothes, or spare parts. You can stack them, move them, and put them on any truck or ship.

**Docker containers work the same way.** A container packages an application with everything it needs to run — the code, the runtime, the libraries, the settings. And it runs the same way on any computer, anywhere.

That's why Docker is the gateway drug of homelabbing. Once you understand containers, everything else becomes easier.

---

## Why This Matters

Containers are the standard way applications are deployed in the modern world. Kubernetes (which powers much of the cloud), Docker Hub (the world's largest container registry), and virtually every modern application — they all use containers.

Learning Docker now means you're learning the most in-demand infrastructure skill in tech. Period.

> **📢 Jargon Alert:** "Container" — Think of it as a lightweight, portable package for running applications. Unlike a virtual machine (which virtualizes the entire computer), a container virtualizes just the operating system. This makes containers much faster to start and lighter on resources.

---

## 🟢 Quick Start

### Step 1: Install Docker

SSH into your server and run these commands:

```bash
# Install required packages
sudo apt install ca-certificates curl gnupg

# Add Docker's official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update and install Docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin
```

**Expected output:** The installation will download and install Docker packages. You'll see progress bars and "Setting up" messages. At the end, Docker should be installed and running.

> **⚠️ The Pitfall:** If you see an error about missing dependencies, run `sudo apt --fix-broken install` first, then retry the Docker installation.

### Step 2: Verify Docker Works

```bash
# Check Docker version
docker --version

# Run your first container
sudo docker run hello-world
```

**Expected output:**
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

Congratulations. You just ran your first container.

### Step 3: Run Docker Without `sudo`

By default, Docker requires `sudo`. Let's fix that so you can run Docker commands as your regular user:

```bash
# Add your user to the docker group
sudo usermod -aG docker $USER

# Apply the change (you'll need to log out and back in, or run this)
newgrp docker
```

Now you can run:
```bash
docker run hello-world
```

Without `sudo`. Much cleaner.

### Step 4: Run a Real Service — Uptime Kuma

Let's deploy something useful. [Uptime Kuma](https://github.com/louislam/uptime-kuma) is a beautiful, self-hosted monitoring tool that tells you when your services are up or down.

```bash
# Create a directory for Kuma's data
mkdir -p ~/uptime-kuma && cd ~/uptime-kuma

# Run Uptime Kuma
docker run -d \
  --name uptime-kuma \
  -p 3001:3001 \
  -v ~/uptime-kuma/data:/app/data \
  --restart unless-stopped \
  louislam/uptime-kuma:1
```

Let's break down what this command does:

```bash
docker run -d \                          # Run in background (detached)
  --name uptime-kuma \                   # Give it a name
  -p 3001:3001 \                         # Map port 3001 on your computer to port 3001 in the container
  -v ~/uptime-kuma/data:/app/data \      # Save data persistently (volume)
  --restart unless-stopped \             # Auto-restart unless you explicitly stop it
  louislam/uptime-kuma:1                 # Use this specific image and version
```

### Step 5: Access Your Service

Open your web browser and go to:
```
http://YOUR_SERVER_IP:3001
```

Replace `YOUR_SERVER_IP` with your server's IP address (the same one from Chapter 2, e.g., `192.168.1.100`).

You should see the Uptime Kuma dashboard. Set up a password on first use, and you're in.

**To verify the container is running:**
```bash
docker ps
```

**Expected output:**
```
CONTAINER ID   IMAGE                      COMMAND          STATUS         PORTS                   NAMES
a1b2c3d4e5f6   louislam/uptime-kuma:1     "./node server"  Up 2 minutes   0.0.0.0:3001->3001/tcp  uptime-kuma
```

---

## 🔵 The Why

### What Just Happened?

When you ran `docker run louislam/uptime-kuma:1`, Docker did several things:

1. **Checked locally** for the `louislam/uptime-kuma:1` image (it wasn't there)
2. **Downloaded it from Docker Hub** (the world's largest container registry — think of it as an "app store" for containers)
3. **Created an isolated environment** with its own filesystem, network, and process space
4. **Started the application** inside that environment
5. **Made port 3001 accessible** from your network

### Why Containers Matter

Imagine you want to install 5 different services on your server. Without containers, each one might need different versions of the same libraries, different configurations, and they might conflict with each other.

With containers: each service runs in its own isolated box. Service A doesn't care what Service B is doing. They can use different versions of everything and never conflict.

```
Without Docker:                    With Docker:
┌─────────────────────┐           ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐
│  Operating System   │           │Kuma  │ │Vault │ │Next │ │Pi-   │ │Jell- │
│  (everything shared)│           │contain│ │contain│ │contain│ │hole  │ │fin   │
│                     │           │  │   │  │  │   │  │  │   │  │contain│  │contain│
└─────────────────────┘           └──────┘ └──────┘ └──────┘ └──────┘ └──────┘
                                    Everything shares the OS, but apps are isolated
```

### Why Uptime Kuma?

We chose Uptime Kuma as your first container because:
- It's lightweight (runs on as little as 256MB RAM)
- It's useful immediately (you'll actually use it)
- It has a nice web interface
- It demonstrates core Docker concepts (ports, volumes, restart policies)

---

## 🟣 Deep Dive

### Docker Architecture

```
┌─────────────────────────────────────────┐
│           Docker CLI (you)              │
└────────────────┬────────────────────────┘
                 │
┌────────────────▼────────────────────────┐
│        Docker Daemon (dockerd)          │
│  ┌──────────┐ ┌──────────┐ ┌────────┐  │
│  │ Images   │ │ Containers│ │Networks│  │
│  │          │ │          │ │        │  │
│  └──────────┘ └──────────┘ └────────┘  │
└─────────────────────────────────────────┘
                 │
┌────────────────▼────────────────────────┐
│        containerd (runtime)             │
└─────────────────────────────────────────┘
```

**Key concepts:**
- **Image:** The template/blueprint (like a class in OOP). It's read-only.
- **Container:** A running instance of an image (like an object instantiated from a class).
- **Volume:** Persistent storage that survives container recreation.
- **Network:** How containers communicate with each other and the outside world.
- **Registry:** Where images are stored (Docker Hub is the default).

### Common Docker Commands

```bash
# List running containers
docker ps

# List all containers (including stopped ones)
docker ps -a

# View container logs
docker logs uptime-kuma

# Stop a container
docker stop uptime-kuma

# Start a stopped container
docker start uptime-kuma

# Restart a container
docker restart uptime-kuma

# Remove a container
docker rm uptime-kuma

# Remove an image
docker rmi louislam/uptime-kuma:1

# View disk usage
docker system df
```

### Docker Compose: Managing Multiple Containers

As you add more services, running individual `docker run` commands gets messy. Docker Compose lets you define your entire stack in a single YAML file.

Create a file called `docker-compose.yml`:

```yaml
# docker-compose.yml
version: "3.8"

services:
  uptime-kuma:
    image: louislam/uptime-kuma:1
    container_name: uptime-kuma
    ports:
      - "3001:3001"
    volumes:
      - ./data:/app/data
    restart: unless-stopped
```

Then manage everything with simple commands:

```bash
# Start all services
docker compose up -d

# Stop all services
docker compose down

# View logs
docker compose logs -f

# Update all images
docker compose pull
docker compose up -d
```

> **💡 Quick Win:** Start using Docker Compose from the beginning. Even for a single service, it makes your setup portable and documentable.

---

## 💼 Career Boost

**What you learned that employers care about:**
- Container runtime (Docker) — the foundation of modern DevOps
- Image management and registry usage
- Container networking (port mapping)
- Persistent storage (volumes)
- Service lifecycle management (start, stop, restart)

**Interview talking point:**
> *"I deployed and managed containerized services using Docker and Docker Compose. I understand container isolation, image management, volume-based persistence, and network configuration. I've deployed monitoring infrastructure (Uptime Kuma) and can demonstrate container lifecycle management."*

**Related job roles:** DevOps Engineer, Site Reliability Engineer, Backend Engineer, Systems Administrator, Cloud Engineer

---

## 🇵🇭 PH Context

### Slow Docker Pulls in the Philippines

Docker Hub can be slow from the Philippines. If downloads are taking forever:

**Option 1: Use a mirror**
```bash
# Edit Docker daemon config
sudo nano /etc/docker/daemon.json
```

Add:
```json
{
  "registry-mirrors": [
    "https://mirror.gcr.io"
  ]
}
```

Then restart Docker:
```bash
sudo systemctl restart docker
```

**Option 2: Download during off-peak hours** (late night or early morning when internet is faster)

**Option 3: Use a larger microSD/SSD** — Some images are 500MB-2GB. With limited storage, be selective about what you pull.

### Hardware Considerations

- **Raspberry Pi:** Docker works but uses ARM images. Make sure the image you want has ARM support. Uptime Kuma, for example, works great on Pi.
- **Old laptops (4GB RAM):** You can run 3-5 lightweight containers comfortably. Avoid running memory-hungry services like Elasticsearch.
- **Storage:** Containers themselves are small (100MB-1GB each), but their data (databases, files) can grow. Plan your storage accordingly.

---

## Stress Test

Now let's prove your Docker setup is solid:

1. **Delete the container** (`docker rm -f uptime-kuma`). Then recreate it with the same `docker run` command from Step 4. Your data should still be there (because of the volume mount at `~/uptime-kuma/data`).
2. **Stop Docker** (`sudo systemctl stop docker`). Wait 10 seconds. Start it again (`sudo systemctl start docker`). Check `docker ps` — if Uptime Kuma is gone, that's expected. Run the `docker run` command again.
3. **Pull a fresh image** (`docker pull louislam/uptime-kuma:1`). Then delete the old one (`docker rmi louislam/uptime-kuma:1`) and run it again. This simulates what happens when Docker Hub updates an image.
4. **Fill up the disk** — not completely, but close. Run `docker system df` to see how much space images and volumes are using. This teaches you what happens when storage runs low.

> **🔥 The Chaos Champion:** Run `docker system prune -f` (NOT with `-a` or `--volumes`). This removes all stopped containers and unused images. Then check `docker ps`. Your running container should still be there. This is safe cleanup. Now try `docker system prune -a --volumes` — this removes EVERYTHING unused, including volumes. **Don't do this on a running homelab.** But run it on a test setup to see what it does. Learn what NOT to do.

---

## What's Next

You've got Docker running and your first service deployed. In Chapter 4, we'll deploy something you'll actually use every day — a password manager, file storage, ad blocker, or media server. Pick one and make it yours.

**Homework:**
1. Practice the Docker commands above
2. Check Uptime Kuma's dashboard and add a monitor for your server's public IP (use [checkip.amazonaws.com](https://checkip.amazonaws.com))
3. Try stopping and starting the container: `docker stop uptime-kuma` then `docker start uptime-kuma`
4. View the logs: `docker logs uptime-kuma`

---

## Go Deeper

- [Docker Documentation](https://docs.docker.com/) — Official docs, very well written
- [Docker Hub](https://hub.docker.com/) — Browse container images
- [Docker Lab](https://lab.docker.com/) — Interactive Docker tutorials
- [Awesome Docker](https://github.com/veggieduck/awesome-docker) — Curated list of Docker resources
