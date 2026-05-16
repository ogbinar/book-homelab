# Commands Reference

_Quick reference for common CLI commands used in this book._

---

## Docker Commands

### Container Management

| Command | What It Does |
|---|---|
| `docker ps` | List running containers |
| `docker ps -a` | List all containers (including stopped) |
| `docker run -d <image>` | Run a container in detached mode |
| `docker stop <name>` | Stop a running container |
| `docker start <name>` | Start a stopped container |
| `docker restart <name>` | Restart a container |
| `docker rm <name>` | Remove a stopped container |
| `docker rm -f <name>` | Force-remove a running container |
| `docker logs <name>` | View container logs |
| `docker logs -f <name>` | Follow logs in real-time |
| `docker exec -it <name> bash` | Open a shell inside a running container |

### Docker Images

| Command | What It Does |
|---|---|
| `docker images` | List downloaded images |
| `docker pull <image>` | Download an image |
| `docker rmi <image>` | Remove an image |
| `docker image prune -a` | Remove all unused images |

### Docker Compose

| Command | What It Does |
|---|---|
| `docker compose up -d` | Start all services in the compose file |
| `docker compose down` | Stop and remove all services |
| `docker compose ps` | List all services and their status |
| `docker compose logs` | View logs for all services |
| `docker compose logs -f <service>` | Follow logs for a specific service |
| `docker compose restart` | Restart all services |
| `docker compose restart <service>` | Restart a specific service |
| `docker compose up -d --force-recreate` | Recreate all containers with new config |
| `docker compose pull` | Pull latest images for all services |
| `docker compose config` | Validate the compose file |

### Docker Network

| Command | What It Does |
|---|---|
| `docker network ls` | List all Docker networks |
| `docker network create <name>` | Create a new network |
| `docker network rm <name>` | Remove a network |

### Docker Volume

| Command | What It Does |
|---|---|
| `docker volume ls` | List all volumes |
| `docker volume create <name>` | Create a new volume |
| `docker volume rm <name>` | Remove a volume |
| `docker volume prune` | Remove all unused volumes |

### Docker System Cleanup

| Command | What It Does |
|---|---|
| `docker system df` | Show Docker disk usage |
| `docker system prune` | Remove unused data |
| `docker system prune -f` | Remove unused data (no confirmation) |
| `docker system prune -a --volumes` | Remove everything unused (careful!) |

---

## Linux System Commands

### File Management

| Command | What It Does |
|---|---|
| `ls` | List files |
| `ls -la` | List all files (including hidden) |
| `cd <directory>` | Change directory |
| `pwd` | Show current directory |
| `mkdir -p <path>` | Create directories (including parents) |
| `rm -rf <path>` | Remove files/directories recursively |
| `cp -r <src> <dst>` | Copy directories recursively |
| `mv <src> <dst>` | Move or rename |
| `nano <file>` | Edit a file with nano editor |
| `cat <file>` | Display file contents |
| `tail -f <file>` | Follow a file in real-time |

### Disk and Storage

| Command | What It Does |
|---|---|
| `df -h` | Show disk usage (human-readable) |
| `du -sh <directory>` | Show directory size |
| `lsblk` | List block devices (disks, partitions) |
| `blkid <device>` | Show device UUIDs |
| `mount <device> <mountpoint>` | Mount a filesystem |
| `sudo blkid` | List all block device UUIDs |

### Memory and CPU

| Command | What It Does |
|---|---|
| `free -h` | Show memory usage |
| `uptime` | Show system uptime and load average |
| `top` | Show real-time process list |
| `htop` | Interactive process viewer (install with `sudo apt install htop`) |

### Network

| Command | What It Does |
|---|---|
| `ip addr show` or `ip a` | Show network interfaces and IPs |
| `ip route \| grep default` | Show default gateway (router IP) |
| `ping <host>` | Test connectivity |
| `curl <url>` | Fetch a URL from the command line |
| `sudo ufw status` | Show firewall status |
| `sudo ufw allow <port>/tcp` | Allow a port through the firewall |
| `sudo ufw enable` | Enable the firewall |
| `sudo lsof -i :<port>` | Show what's using a specific port |

### Network Scanning

| Command | What It Does |
|---|---|
| `sudo apt install nmap` | Install nmap |
| `sudo nmap -sn <range>` | Scan for active devices |
| `nmap -p <ports> <host>` | Scan specific ports |

### Package Management (Ubuntu/Debian)

| Command | What It Does |
|---|---|
| `sudo apt update` | Update package list |
| `sudo apt upgrade` | Upgrade all packages |
| `sudo apt install <package>` | Install a package |
| `sudo apt remove <package>` | Remove a package |
| `apt list --upgradable` | Show upgradable packages |

### Cron (Scheduled Tasks)

| Command | What It Does |
|---|---|
| `crontab -e` | Edit your cron jobs |
| `crontab -l` | List your cron jobs |
| `crontab -r` | Remove all cron jobs |

### Systemd (Services)

| Command | What It Does |
|---|---|
| `sudo systemctl status <service>` | Check service status |
| `sudo systemctl start <service>` | Start a service |
| `sudo systemctl stop <service>` | Stop a service |
| `sudo systemctl restart <service>` | Restart a service |
| `sudo systemctl enable <service>` | Start service on boot |
| `sudo systemctl disable <service>` | Don't start service on boot |

### SSH

| Command | What It Does |
|---|---|
| `ssh username@host` | Connect to a remote server |
| `ssh -i <key> username@host` | Connect using a specific key |
| `ssh-keygen -t ed25519` | Generate a new SSH key |
| `ssh-copy-id username@host` | Copy your key to a remote server |
| `scp <file> username@host:/path` | Copy a file to a remote server |

### Backup and Archive

| Command | What It Does |
|---|---|
| `tar czf <archive.tar.gz> <source>` | Create a compressed archive |
| `tar xzf <archive.tar.gz>` | Extract a compressed archive |
| `tar tzf <archive.tar.gz>` | List contents of an archive |

---

## Quick Reference: Chapter Command Map

| Chapter | Key Commands |
|---|---|
| Ch 1 | `ssh`, `sudo apt update`, `sudo apt upgrade` |
| Ch 2 | `ssh-keygen`, `ssh-copy-id`, `sudo apt install` |
| Ch 3 | `docker run`, `docker ps`, `docker logs` |
| Ch 4 | `docker run`, `docker ps`, `sudo lsof -i` |
| Ch 5 | `docker inspect`, `docker update`, `docker system prune` |
| Ch 6 | `ip addr`, `ip route`, `sudo nmap`, `sudo ufw` |
| Ch 7 | `docker compose up`, `docker compose restart` |
| Ch 8 | `docker compose up -d`, `docker compose down`, `docker compose ps` |
| Ch 9 | `tar czf`, `tar xzf`, `crontab -e`, `lsblk` |
| Ch 10 | `crontab -e`, `docker compose pull`, `docker compose up -d` |
| Ch 11 | `docker compose up -d`, `curl` |
| Ch 12 | `ssh-keygen`, `ssh-copy-id`, `sudo ufw`, `sudo fail2ban-client` |
| Ch 13 | `tailscale up`, `cloudflared tunnel create`, `cloudflared tunnel run` |
| Ch 14 | `git init`, `git add`, `git commit`, `git push` |

---

> **💡 Quick Win:** Save this page as a bookmark. When you forget a command, come back here. Even experienced admins look up commands all the time — there's no shame in it.
