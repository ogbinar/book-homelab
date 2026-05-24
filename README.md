# Bahay Bahayan: Build Your First Homelab

> **A Filipino-context-aware guide to building your first homelab — from ₱0 to a fully automated, monitored, and secure infrastructure.**

**Official repository:** https://github.com/ogbinar/book-homelab

**Public site:** https://ogbinar.com/book-homelab/

**README:** https://github.com/ogbinar/book-homelab/blob/main/README.md

**Book license:** [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

**Code license:** [MIT](https://opensource.org/license/mit)

---

## What Is This?

This is a step-by-step book for Filipino tech professionals and beginners who want to build their own homelab — a personal infrastructure that runs on hardware you own, in your home, with full control.

The book grows like a school ladder from `Prep` to `Postdoc`, but the real organizing idea stays home-shaped: power, water, locks, front desk, utility cabinet, game room, and study room.

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
| 7 | The Reverse Proxy + Remote Access | Caddy, SSL, clean URLs |
| 8 | Multiple Services, One System | Docker Compose stack, IaC basics |
| 9 | Your Data, Safe | 3-2-1 backup strategy, Restic |
| 10 | Automation — Work Less | Watchtower, cron, maintenance scripts |
| 11 | Monitoring — Eyes on Everything | Prometheus, Grafana, node-exporter |
| 12 | Security — Don't Get Hacked | SSH keys, UFW, fail2ban, container isolation |
| 13 | Your Homelab Portfolio | Documentation, GitHub setup, optional career translation |

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

- The early chapters are the `Prep` and `Kinder` wins.
- The middle chapters move into `High School`, `College`, and `Masters` territory.
- The advanced ideas like game hosting, local hosted LLMs, and Dokploy-style service management belong later or in future expansion.

- **🟢 Quick Start** — The hands-on steps. Do these.
- **🔵 The Why** — The concepts behind what you're doing.
- **🟣 Deep Dive** — Advanced topics for those who want more.
- **💼 Career Boost** — Optional career value when it helps the reader.
- **🇵🇭 PH Context** — Filipino-specific considerations (costs, ISPs, hardware).
- **📢 Jargon Alert** — Tech terms explained in plain language.

## Quick Start

1. Read Chapter 1 to understand what a homelab is and why you might want one
2. Follow Chapter 2 to set up your first server (₱0 if you have an old laptop)
3. Follow the chapters in order — each one builds on the last
4. By Chapter 13, you'll have a portfolio to show employers

## Filipino Context

This book is written for Filipinos, by Filipinos (well, for Filipinos). We include:

- **Local hardware prices** (₱) from Shopee, Lazada, Computer City, Facebook Marketplace
- **Local ISP considerations** (PLDT, Globe, DITO, Converge)
- **Power outage strategies** (brownouts, Meralco rates, UPS recommendations)
- **Flood considerations** (common in Metro Manila and other PH cities)
- **Job market context** (remote work salaries, PH tech recruitment)

## Contributing

Contributions are welcome!

- Book prose, diagrams, and documentation are licensed under CC BY-SA 4.0.
- Code snippets, scripts, and reusable code artifacts are licensed under MIT.

1. Fork and clone this repository
2. Follow the structure used in the existing chapters under `docs/chapters/`
3. Keep the Taglish tone natural, not forced
4. Include PH context where it helps the reader
5. Submit a pull request

## Report an Error

If you find a factual error, broken link, typo, or unclear instruction:

1. Open a GitHub issue at https://github.com/ogbinar/book-homelab/issues
2. Include the chapter, section heading, and the exact line or quote that needs fixing
3. Describe the correction you want to see
4. If you already know the fix, submit a pull request instead of waiting for a maintainer

Keep reports specific. "Chapter 8, Docker Compose example, missing volume mount" is actionable. "Something is wrong" is not.

## Directory Structure

```
book-homelab/
├── README.md
├── agents.md
├── design/
│   └── book-design.md
├── docs/
│   ├── index.md
│   ├── launch-readiness-report.md
│   ├── part-i.md
│   ├── part-ii.md
│   ├── chapters/
│   │   ├── chapter-01.md
│   │   ├── chapter-02.md
│   │   ├── chapter-03.md
│   │   ├── chapter-04.md
│   │   ├── chapter-05.md
│   │   ├── chapter-06.md
│   │   ├── chapter-07.md
│   │   ├── chapter-08.md
│   │   ├── chapter-09.md
│   │   ├── chapter-10.md
│   │   ├── chapter-11.md
│   │   ├── chapter-12.md
│   │   └── chapter-13.md
│   └── appendix/
│       ├── author.md
│       ├── commands.md
│       ├── community.md
│       ├── errors.md
│       ├── glossary.md
│       ├── hardware.md
│       └── services.md
├── mkdocs.yml
```

Internal planning notes live in the workspace, but they are not part of the published book tree.

## License

This book and its documentation are licensed under the Creative Commons
Attribution 4.0 International License.

You are free to:
- **Share** — Copy and redistribute the material in any medium or format
- **Adapt** — Remix, transform, and build upon the material for any purpose,
  even commercially

Under the following terms:
- **Attribution** — You must give appropriate credit
- No additional restrictions — You may not apply legal terms or technological
  measures that legally restrict others from doing anything the license permits

See [LICENSE](LICENSE) for the full text of the book license.

Code snippets and scripts in this repository are licensed under MIT unless a
file says otherwise.

---

> **📢 Remember:** Your homelab doesn't need to be impressive. It needs to be yours. Start small. Be stubborn. Have fun.
