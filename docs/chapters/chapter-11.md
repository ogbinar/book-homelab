# Chapter 11: Monitoring — Eyes on Everything

## What You'll Build
A professional monitoring dashboard using Prometheus and Grafana that tracks your server's health, container status, and service availability — with alerts that notify you when something goes wrong.

## How Long It Takes
2 hours (including reading and configuration)

## What You Need
- Your homelab stack running (Chapters 1-10)
- At least 2GB free RAM (Prometheus + Grafana need memory)

---

## The Story

You have 7 services running on your server. But here's the question: **how do you know they're all working right now?**

You could check each one manually — open 7 browser tabs, verify each one loads. That's... not sustainable.

Or you could have a single dashboard that shows you everything at a glance:
- Is the server online?
- Are all containers running?
- How much disk space is left?
- How much memory is being used?
- Is the internet connection stable?
- How many requests is each service handling?

This chapter builds that dashboard.

> **📢 Jargon Alert:** "Observability" — The ability to understand the internal state of a system by examining its outputs (metrics, logs, traces). Monitoring is collecting the data. Observability is understanding what it means.

---

## Why This Matters

In professional infrastructure, there's a saying: **"If you can't measure it, you can't manage it."**

Monitoring tells you:
- **Now:** Is everything working? (dashboard)
- **Recently:** What happened yesterday? (metrics history)
- **Soon:** What's going to break? (trends and alerts)

Without monitoring, you're flying blind. With it, you're in control.

---

## 🟢 Quick Start

### Step 1: Add Prometheus and Grafana to Your Stack

```bash
# In your docker-compose.yml, add these services:

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    restart: unless-stopped
    ports:
      - "9090:9090"
    volumes:
      - ./config/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - ./data/prometheus:/prometheus
    networks:
      - homelab

  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    restart: unless-stopped
    ports:
      - "9100:9100"
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    command:
      - '--path.procfs=/host/proc'
      - '--path.sysfs=/host/sys'
      - '--path.rootfs=/rootfs'
    networks:
      - homelab

  grafana:
    image: grafana/grafana-oss:latest
    container_name: grafana
    restart: unless-stopped
    ports:
      - "3000:3000"
    volumes:
      - ./data/grafana:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=${GRAFANA_PASSWORD:-admin}
    networks:
      - homelab
```

### Step 2: Create Prometheus Configuration

```bash
mkdir -p ~/homelab/config/prometheus
nano ~/homelab/config/prometheus/prometheus.yml
```

```yaml
# prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  # Prometheus itself
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  # Node exporter (server metrics)
  - job_name: 'node'
    static_configs:
      - targets: ['node-exporter:9100']

  # cAdvisor (container metrics)
  - job_name: 'cadvisor'
    static_configs:
      - targets: ['cadvisor:8080']
```

### Step 3: Add cAdvisor to Your Stack

cAdvisor (Container Advisor) collects container metrics:

```yaml
  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    container_name: cadvisor
    restart: unless-stopped
    ports:
      - "9101:8080"
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
      - /dev/disk/:/dev/disk:ro
    devices:
      - /dev/kmsg
    networks:
      - homelab
```

### Step 4: Start Everything

```bash
cd ~/homelab
docker compose up -d
```

### Step 5: Set Up Grafana

1. Open `http://YOUR_SERVER_IP:3000` in your browser
2. Log in with `admin` / `admin` (you'll be prompted to change the password)
3. Add Prometheus as a data source:
   - Click ⚙️ (Settings) → Data Sources → Add data source
   - Select **Prometheus**
   - URL: `http://prometheus:9090`
   - Click **Save and Test**

### Step 6: Import a Dashboard

Grafana has pre-built dashboards. The most popular one for Docker/server monitoring is **Dashboard ID: 893** (Docker and System):

1. Click + (Import) → Import via grafana.com
2. Enter `893`
3. Select your Prometheus data source
4. Click **Load**
5. Click **Import**

You should now see a beautiful dashboard showing:
- CPU usage
- Memory usage
- Disk usage
- Network traffic
- Container metrics

> **💡 Quick Win:** If Dashboard 893 doesn't work, try Dashboard **1860** (Node Exporter Full) or **8172** (Prometheus Stats).

---

## 🔵 The Why

### The Monitoring Stack Explained

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  Metrics    │────▶│  Prometheus  │────▶│   Grafana   │
│  Sources    │     │  (Storage)   │     │  (Visualize)│
└─────────────┘     └──────────────┘     └─────────────┘
       │                    │                    │
       │                    │                    │
  ┌────▼────┐          ┌────▼────┐          ┌────▼────┐
  │Node     │          │Prom     │          │Dashboard│
  │Exporter │          │Server   │          │Alerts   │
  └─────────┘          └─────────┘          └─────────┘
  ┌────┴────┐
  │cAdvisor │
  └─────────┘
```

**Each component's role:**
- **Node Exporter:** Collects server-level metrics (CPU, memory, disk, network)
- **cAdvisor:** Collects container-level metrics (per-container CPU, memory, network)
- **Prometheus:** Stores all metrics as time-series data
- **Grafana:** Visualizes the data in dashboards

### Alerting

Prometheus can alert when things go wrong. Add alerting rules:

```bash
mkdir -p ~/homelab/config/prometheus/rules
nano ~/homelab/config/prometheus/rules/alerts.yml
```

```yaml
# alerts.yml
groups:
  - name: homelab-alerts
    rules:
      # Alert if disk usage exceeds 85%
      - alert: HighDiskUsage
        expr: node_filesystem_avail_bytes{mountpoint="/"} / node_filesystem_size_bytes{mountpoint="/"} * 100 < 15
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High disk usage on {{ $labels.instance }}"
          description: "Disk usage is above 85% (current: {{ $value }}%)"

      # Alert if a container is down
      - alert: ContainerDown
        expr: up{job="cadvisor"} == 0
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "Container {{ $labels.name }} is down"
```

Reference the rules in `prometheus.yml`:
```yaml
rule_files:
  - '/etc/prometheus/rules/alerts.yml'
```

### Simple Alternative: Uptime Kuma

If Prometheus+Grafana feels overwhelming, Uptime Kuma (which you already have) is a perfectly valid monitoring solution:

1. Open Uptime Kuma
2. Add HTTP/Ping monitors for each service
3. Set up notifications (Telegram, email, Discord)
4. Done

**Uptime Kuma vs. Prometheus+Grafana:**

| Feature | Uptime Kuma | Prometheus+Grafana |
|---|---|---|
| Setup difficulty | ⭐ Easy | ⭐⭐⭐ Hard |
| Real metrics | No (up/down only) | Yes (CPU, RAM, disk, etc.) |
| Alerts | Yes | Yes |
| History | Limited | Unlimited |
| Learning curve | Low | Medium |
| Best for | Beginners | Intermediate+ |

> **🧠 The Deep Dive:** Prometheus uses a pull-based model (it scrapes metrics from your services). This is different from push-based monitoring where services send metrics to a central server. Pull-based is simpler for homelabs but requires all services to expose a metrics endpoint.

---

## 💼 Career Boost

**What you learned that employers care about:**
- Observability stack deployment (Prometheus + Grafana)
- Time-series database concepts
- Dashboard design and visualization
- Alerting and notification systems
- Container monitoring

**Interview talking point:**
> *"I designed and deployed a full observability stack using Prometheus for metrics collection, Grafana for visualization, and cAdvisor for container-level monitoring. The stack tracks 50+ metrics across 7 services with automated alerting via Telegram, achieving complete infrastructure visibility."*

---

## 🇵🇭 PH Context

### Resource Considerations

| Service | RAM Usage | CPU Usage |
|---|---|---|
| Prometheus | 200-500MB | Low |
| Grafana | 100-200MB | Low |
| Node Exporter | ~10MB | Minimal |
| cAdvisor | 50-100MB | Low |

**Total: ~400-800MB RAM** — on a system with 4GB+ RAM, this is negligible.

### Mobile-First Dashboards

Grafana dashboards are mobile-responsive. Set up your dashboard as a bookmark on your phone's home screen for instant access.

### Data-Saver Considerations

Grafana dashboards load metrics via API calls. If you're monitoring from mobile data:
- Set `scrape_interval` to 60s instead of 15s in prometheus.yml
- Grafana caches dashboard data, so refreshing doesn't always fetch new data

---

## Stress Test

Now let's prove your monitoring works:

1. **Stop a service** (e.g., `docker compose down vaultwarden`). Watch your Grafana dashboard. Node-exporter should show the container as stopped. Uptime Kuma should show the service as down. This proves your monitoring catches failures.
2. **Fill up the disk** — not completely, but to 80%+. Watch Grafana's disk usage graph. If you have alerts set up, you should get notified. This proves your monitoring catches resource exhaustion.
3. **Stop Prometheus** (`docker compose stop prometheus`). Grafana should show "no data" or an error — because Prometheus is the data source. This proves the dependency chain: services → Prometheus → Grafana.
4. **Simulate high CPU** — run `stress --cpu 4` on your server (install with `sudo apt install stress`). Watch Grafana's CPU usage graph spike. This proves your monitoring captures real-time changes.

> **🔥 The Chaos Champion:** Delete your Prometheus data directory (`rm -rf data/prometheus/*`). Restart Prometheus. The dashboard will be empty — because all historical data is gone. This teaches you an important lesson: monitoring data is not backup data. Your metrics are useful for trends, but they don't protect your services. That's why backups (Chapter 9) and monitoring (this chapter) serve different purposes.

---

## What's Next

You can now see everything happening in your homelab. In Chapter 12, we'll secure it — because a monitored but unprotected homelab is just a vulnerable one.

**Homework:**
1. Deploy Prometheus, Grafana, node-exporter, and cAdvisor
2. Import a monitoring dashboard
3. Set up at least one alert rule
4. Access your dashboard from your phone

---

## Go Deeper

- [Prometheus Documentation](https://prometheus.io/docs/) — Complete reference
- [Grafana Documentation](https://grafana.com/docs/) — Dashboard and alerting guide
- [cAdvisor Documentation](https://github.com/google/cadvisor) — Container metrics
- [Grafana Dashboard Library](https://grafana.com/grafana/dashboards/) — 1000+ pre-built dashboards
- [Uptime Kuma](https://github.com/louislam/uptime-kuma) — Simple monitoring alternative
