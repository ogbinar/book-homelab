# Service Directory

_Self-hosted services and tools recommended for your homelab._

---

## Services Covered in This Book

> **Quick Reference:** See **[Chapter 4](chapters/chapter-04.md)** for deployment, **[Chapter 7](chapters/chapter-07.md)** for reverse proxy setup, **[Chapter 11](chapters/chapter-11.md)** for monitoring.

### Uptime Kuma — Monitoring
| Detail | Info |
|---|---|
| **Purpose** | Monitor service uptime and get alerts when things go down |
| **Docker Image** | `louislam/uptime-kuma:1` |
| **Default Port** | 3001 |
| **RAM Usage** | ~100-200MB |
| **Best For** | Beginners — easiest monitoring solution |
| **Docs** | https://github.com/louislam/uptime-kuma |

### Vaultwarden — Password Manager
| Detail | Info |
|---|---|
| **Purpose** | Self-hosted password manager (Bitwarden-compatible) |
| **Docker Image** | `vaultwarden/server:latest` |
| **Default Port** | 8080 |
| **RAM Usage** | ~20-50MB |
| **Best For** | Replacing 1Password/LastPass |
| **Docs** | https://docs.vaultwarden.org/ |

### Nextcloud — File Storage & Collaboration
| Detail | Info |
|---|---|
| **Purpose** | Cloud storage, file sharing, calendar, contacts, documents |
| **Docker Image** | `nextcloud:latest` |
| **Default Port** | 8081 |
| **RAM Usage** | ~300-500MB |
| **Best For** | Replacing Google Drive/Dropbox |
| **Docs** | https://docs.nextcloud.com/ |

### Pi-hole — Ad Blocking
| Detail | Info |
|---|---|
| **Purpose** | DNS-level ad blocking for your entire network |
| **Docker Image** | `pihole/pihole:latest` |
| **Default Port** | 53 (DNS), 8082 (Admin) |
| **RAM Usage** | ~50-100MB |
| **Best For** | Blocking ads on all devices simultaneously |
| **Docs** | https://docs.pi-hole.net/ |

### Jellyfin — Media Server
| Detail | Info |
|---|---|
| **Purpose** | Stream your movies, TV shows, music, and photos |
| **Docker Image** | `jellyfin/jellyfin:latest` |
| **Default Port** | 8096 |
| **RAM Usage** | ~200-500MB (more if transcoding) |
| **Best For** | Replacing Netflix for your own content |
| **Docs** | https://jellyfin.org/docs/ |

### Caddy — Reverse Proxy
| Detail | Info |
|---|---|
| **Purpose** | Route traffic to your services using clean URLs with auto-SSL |
| **Docker Image** | `caddy:latest` |
| **Default Port** | 80, 443 |
| **RAM Usage** | ~10-30MB |
| **Best For** | Clean URLs, automatic HTTPS |
| **Docs** | https://caddyserver.com/docs/ |

### Prometheus — Metrics Collection
| Detail | Info |
|---|---|
| **Purpose** | Collect and store time-series metrics from your services |
| **Docker Image** | `prom/prometheus:latest` |
| **Default Port** | 9090 |
| **RAM Usage** | 200-500MB |
| **Best For** | Advanced monitoring and alerting |
| **Docs** | https://prometheus.io/docs/ |

### Grafana — Visualization
| Detail | Info |
|---|---|
| **Purpose** | Create dashboards from Prometheus metrics |
| **Docker Image** | `grafana/grafana-oss:latest` |
| **Default Port** | 3000 |
| **RAM Usage** | 100-200MB |
| **Best For** | Beautiful, interactive dashboards |
| **Docs** | https://grafana.com/docs/ |

### Node Exporter — Server Metrics
| Detail | Info |
|---|---|
| **Purpose** | Expose server hardware metrics (CPU, RAM, disk, network) |
| **Docker Image** | `prom/node-exporter:latest` |
| **Default Port** | 9100 |
| **RAM Usage** | ~10MB |
| **Best For** | Server-level monitoring |
| **Docs** | https://github.com/prometheus/node_exporter |

### cAdvisor — Container Metrics
| Detail | Info |
|---|---|
| **Purpose** | Expose per-container resource usage metrics |
| **Docker Image** | `gcr.io/cadvisor/cadvisor:latest` |
| **Default Port** | 8080 |
| **RAM Usage** | 50-100MB |
| **Best For** | Container-level monitoring |
| **Docs** | https://github.com/google/cadvisor |

### Watchtower — Auto Updates
| Detail | Info |
|---|---|
| **Purpose** | Automatically update Docker containers when new images are available |
| **Docker Image** | `containrrr/watchtower:latest` |
| **Default Port** | None (runs in background) |
| **RAM Usage** | ~10-20MB |
| **Best For** | Keeping all containers up to date |
| **Docs** | https://containrrr.dev/watchtower/ |

---

## Bonus: Services to Explore Next

These aren't covered in the book but are great additions to a homelab:

| Service | Purpose | Docker Image | Port | Why Add It |
|---|---|---|---|---|
| **Gitea** | Self-hosted Git | `gitea/gitea:latest` | 3000 | Your own GitHub |
| **Homepage** | Dashboard | `gethomepage/homepage:latest` | 3000 | Beautiful homelab dashboard |
| **FileBrowser** | Web file manager | `filebrowser/filebrowser:latest` | 80 | Web-based file management |
| **Vault** | Secrets management | `hashicorp/vault:latest` | 8200 | Enterprise-grade secrets |
| **Vaultwarden** | Password manager | `vaultwarden/server:latest` | 8080 | Already covered in Ch 4 |
| **MinIO** | S3-compatible storage | `minio/minio:latest` | 9000 | Your own cloud storage backend |
| **Memos** | Note-taking | `neko8657/memos:latest` | 5230 | Simple, private notes |
| **Outline** | Knowledge base | `getoutline/outline:server` | 3000 | Your own Wikipedia |
| **Paperless-NG** | Document management | `paperless/paperless:latest` | 8000 | Scan and organize documents |
| **Home Assistant** | Smart home | `homeassistant/home-assistant:latest` | 8123 | Automate your home |
| **Plausible** | Analytics | `plausible/analytics:latest` | 8000 | Privacy-friendly Google Analytics alternative |
| **Radarr/Sonarr** | Media automation | `linuxserver/radarr:latest` | 7878 | Auto-download movies/shows |

---

## Choosing the Right Service

Not every service is right for every homelab. Use this guide:

**If you want to save money:** Start with Vaultwarden (passwords) or Pi-hole (ads).

**If you want to learn:** Start with Uptime Kuma (monitoring) — it shows you what's running.

**If you have extra storage:** Start with Nextcloud (files) or Jellyfin (media).

**If you want to look professional:** Start with Caddy (reverse proxy) — clean URLs impress people.

**If you want to monitor everything:** Start with Prometheus + Grafana.

> **💸 The Lean Path:** You don't need all of these. Start with ONE service that solves a real problem for you. Add more when you need them. A homelab with one useful service is better than a homelab with ten unused ones.
