# Bahay Bahayan: Build Your First Homelab

> **A Filipino-context-aware guide to building your first homelab — from ₱0 to a fully automated, monitored, and secure infrastructure.**

**License:** [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

---

## What Is This?

This is a step-by-step book for Filipino tech professionals and beginners who want to build their own homelab — a personal infrastructure that runs on hardware you own, in your home, with full control.

No cloud subscriptions. No terms of service. No "we've decided to change our pricing."

Just you, your hardware, and whatever software you want to run on it.

> **Bahay** (home) + **-an** (place) = **Bahay Bahayan** — a place to put things. A home for your infrastructure.

## Who Is This For?

- **Beginners** who have never touched a server
- **Tech professionals** who want hands-on infrastructure experience
- **Filipinos** who want to understand homelabbing in a local context (₱ prices, PH hardware, local ISPs)
- **Anyone** who's tired of paying for subscriptions they barely use

## What You'll Learn

| Chapter | Topic | Skills |
|---------|-------|--------|
| 1 | What Even Is a Homelab? | Philosophy, mindset, your "why" |
| 2 | Your First Server | Hardware selection, Ubuntu Server install, SSH |
| 3 | Hello, Container | Docker installation, containers, first service |
| 4 | Something Useful | Deploy Vaultwarden, Nextcloud, Pi-hole, or Jellyfin |
| 5 | Keeping It Alive | Auto-restart, backups, restore procedures |
| 6 | Your First Network | Static IPs, DNS, ports, routers |
| 7 | The Reverse Proxy | Caddy, SSL, clean URLs |
| 8 | Multiple Services, One System | Docker Compose stack, IaC basics |
| 9 | Your Data, Safe | 3-2-1 backup strategy, Restic |
| 10 | Automation — Work Less | Watchtower, cron, maintenance scripts |
| 11 | Monitoring — Eyes on Everything | Prometheus, Grafana, node-exporter |
| 12 | Security — Don't Get Hacked | SSH keys, UFW, fail2ban, container isolation |
| 14 | Your Homelab Portfolio | CV translation, GitHub setup, career acceleration |

**Total time:** ~15-20 hours spread over 2-4 weeks.

## Tech Stack

- **OS:** Ubuntu Server 24.04 LTS
- **Container Runtime:** Docker + Docker Compose
- **Reverse Proxy:** Caddy
- **DNS/Ad Blocking:** Pi-hole
- **Monitoring:** Uptime Kuma, Prometheus, Grafana
- **Services:** Vaultwarden, Nextcloud, Jellyfin
- **Backup:** rsync, Restic

## How to Read This Book

Read in order. Each chapter builds on the previous one.

- **🟢 Quick Start** — The hands-on steps. Do these.
- **🔵 The Why** — The concepts behind what you're doing.
- **🟣 Deep Dive** — Advanced topics for those who want more.
- **💼 Career Boost** — How this translates to career value.
- **🇵🇭 PH Context** — Filipino-specific considerations (costs, ISPs, hardware).
- **📢 Jargon Alert** — Tech terms explained in plain language.

## Quick Start

1. Read Chapter 1 to understand what a homelab is and why you might want one
2. Follow Chapter 2 to set up your first server (₱0 if you have an old laptop)
3. Follow the chapters in order — each one builds on the last
4. By Chapter 14, you'll have a portfolio to show employers

## Filipino Context

This book is written for Filipinos, by Filipinos (well, for Filipinos). We include:

- **Local hardware prices** (₱) from Shopee, Lazada, Computer City, Facebook Marketplace
- **Local ISP considerations** (PLDT, Globe, DITO, Converge)
- **Power outage strategies** (brownouts, Meralco rates, UPS recommendations)
- **Flood considerations** (common in Metro Manila and other PH cities)
- **Job market context** (remote work salaries, PH tech recruitment)

## Contributing

Contributions are welcome! This book is licensed under CC BY-SA 4.0.

1. Fork and clone this repository
2. Follow the chapter template in `templates/chapter-template.md`
3. Maintain the Taglish tone (natural mix of Filipino + English)
4. Include PH context where relevant
5. Submit a pull request

## Directory Structure

```
book-homelab/
├── README.md                  # This file
├── agents.md                  # Style guide and manifesto
├── design/
│   └── book-design.md         # Full book structure and design spec
├── ideas.md                   # Brainstorming and user journey
├── research/
│   └── homelab-ecosystems-research.md
├── professional-profiles-matrix.md
├── templates/
│   └── chapter-template.md    # Template for new chapters
└── chapters/
    ├── part-i-bahay/          # Chapters 1-6 (Building the foundation)
    │   ├── chapter-01-what-is-homelab.md
    │   ├── chapter-02-your-first-server.md
    │   ├── chapter-03-hello-container.md
    │   ├── chapter-04-something-useful.md
    │   ├── chapter-05-keeping-it-alive.md
    │   └── chapter-06-your-first-network.md
    └── part-ii-lumaki/        # Chapters 7-14 (Growing your lab)
        ├── chapter-07-reverse-proxy.md
        ├── chapter-08-multiple-services-one-system.md
        ├── chapter-09-your-data-safe.md
        ├── chapter-10-automation-work-less.md
        ├── chapter-11-monitoring-eyes-on-everything.md
        ├── chapter-12-security-dont-get-hacked.md
        └── chapter-14-homelab-portfolio.md
```

## License

This work is licensed under the Creative Commons Attribution-ShareAlike 4.0 International License.

You are free to:
- **Share** — Copy and redistribute the material in any medium or format
- **Adapt** — Remix, transform, and build upon the material for any purpose

Under the following terms:
- **Attribution** — You must give appropriate credit
- **ShareAlike** — If you remix, transform, or build upon the material, you must distribute your contributions under the same license

See [LICENSE](LICENSE) for the full text.

---

> **📢 Remember:** Your homelab doesn't need to be impressive. It needs to be yours. Start small. Be stubborn. Have fun.
