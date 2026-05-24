# Book Design Document: "Bahay Bahayan: Build Your First Homelab"

> **Working Title:** *Bahay Bahayan: Build Your First Homelab*  
> **Subtitle:** *Start Small. Learn Fast. Build What's Yours.*  
> **Target Audience:** Absolute beginners in the Philippines — age 16 to 60, any background  
> **Format:** Digital-first (web + PDF), progressive disclosure, modular chapters  
> **Language:** **Taglish** (natural mix of Filipino + English, code/tech terms in English)  
> **Scope:** **Current manuscript** — 13 chapters (Parts I + II plus capstone Chapter 13), Part III reserved for future expansion  
> **Tone:** Friendly, opinionated, anti-gatekeeping, Taglish, Filipino-context-aware, home-shaped, slightly formal when needed
> **License:** **Open source** — free forever, CC BY-SA 4.0, community contributions welcome

---

## 1. Core Principles

- **Household first.** Every idea should map back to a real home, household need, or home constraint.
- **Practical before impressive.** Useful beats flashy. Reliability beats novelty.
- **Filipino reality first.** Brownouts, CGNAT, family sharing, heat, noise, reused hardware, and budget pressure are not edge cases here.
- **One narrator.** The voice is a guiding kuya: calm, slightly formal, practical, and never shamey.
- **Taglish should sound natural.** Use it where a real Filipino speaker would, not as decoration.
- **Advanced topics must stay home-shaped.** Game hosting, Dokploy-style operations, and local hosted LLMs are valid only when they still feel like part of the house.
- **Career value is a bonus, not the product.** The book may help with work, but it is not a career-coaching manual.
- **Bahala Na, used carefully.** If named, it should mean brave action with preparation and responsibility, not reckless improvisation.

---

## 2. Core Design Philosophy

### 1.1 What This Book Is NOT

This book is **not**:
- A reference manual for Linux commands
- A Kubernetes certification prep guide
- A hardware buying guide with specs and benchmarks
- An academic treatise on distributed systems
- A "build exactly this" instruction manual

### 1.2 What This Book IS

This book is:
- **A guided exploration** — "Here's what's possible, here's how to start, here's where it goes"
- **A confidence builder** — "You don't need to be a genius. You need curiosity and 30 minutes a week"
- **A Filipino-first guide** — ₱ prices, local hardware markets, PH internet realities, Taglish narration
- **A home-first systems book** — every advanced topic should still map to a real household need, constraint, or room
- **A values-driven journey** — privacy, ownership, learning, community
- **A project-based learning system** — every chapter produces something you can touch and use
- **A home-shaped learning system** — the metaphor stays close to the house so the reader can remember and reuse it

### 2.3 The Guiding Kuya Voice

The narrator should sound like a practical older sibling who has already made the mistakes, cleaned up the mess, and now wants to save the reader time.

- Be calm, not dramatic.
- Be direct, not cold.
- Be helpful, not preachy.
- Explain the reason once, then move on.
- Respect the reader's budget, time, and family situation.
- Encourage progress under uncertainty without making a joke out of risk.

When the book uses Filipino values explicitly, it should do so naturally. `Bahala Na` can appear as a grounded form of courage if the context calls for it, but it should never become a slogan.

### 2.4 The Learning Ladder

The book should progress like a school ladder, but the point is maturity, not status.

| Level | Meaning in the Book | Typical Feel |
|---|---|---|
| Prep | One thing works | "I can make something run." |
| Kinder | One thing is useful at home | "This helps the household." |
| Elementary | Small, repeatable habits | "I can do this again." |
| High School | Multiple services start to connect | "I can manage a small system." |
| College | Tradeoffs, structure, and coordination | "I can design for other people too." |
| Masters | Resilience, policy, and sustainability | "I can keep this running through real-life problems." |
| PhD | Edge cases, deeper architecture, serious constraints | "I can reason about the weird stuff." |
| Postdoc | Experimental, optional, future-facing | "I am exploring what comes next." |

Use this ladder to decide what belongs in the core manuscript, what belongs in deep dives, and what should wait for Part III.

### 2.5 The Household-Service Metaphor

To keep *Bahay Bahayan* grounded, technical ideas should feel like parts of a house.

| Homelab Concept | Household Analogy | Why It Helps |
|---|---|---|
| Power resilience | UPS, outlet, and backup light | Makes brownouts and safe shutdowns concrete |
| Reverse proxy | Front desk | Explains routing without abstract networking first |
| Backup strategy | Water tank / pantry reserve | Makes durability and restore planning visual |
| Monitoring | Smoke detector / house alerts | Makes uptime and notifications feel domestic |
| Security | Locks, gates, and house rules | Keeps the mental model accessible |
| Docker / Compose | Utility cabinet / toolbox | Shows that services are organized, not magical |
| Game servers | Game room / arcade | Makes hosted play feel like a real household use case |
| Dokploy-style control | Operations desk | Shows the jump from manual commands to managed services |
| Local LLMs | Study room / private assistant desk | Frames AI as a private household helper, not a cloud dependency |

The goal is not to force one metaphor everywhere. The goal is to keep the book close to home so even advanced topics still feel like part of the same place.

### 2.6 The "Bahay Bahayan" Name

**Why "Bahay Bahayan"?** From the Filipino root word "bahay" (home) + "-an" (place), meaning "a place to put things." It signals:

- **Filipino identity** — not a translated Western guide
- **Warmth and approachability** — it sounds like a place, not a textbook
- **Hands-on spirit** — your digital home where you keep your services, your data, your projects
- **Community** — a space that welcomes builders at every level
- **Simplicity** — no jargon, no gatekeeping. Just a place to build.

The name is playful and memorable — the repetition of "bahay" makes it stick. It's deeply Filipino without being exclusionary, and it fits the book's mission: making homelabbing feel like building your own digital home, not joining a technical club.

---

## 2. Target Reader Personas

Based on our research, this book serves **five reader archetypes** — the book should speak to all of them:

### 2.1 The Student (Age 16-24)
- **Budget:** ₱5,000-₱15,000 (allowance + part-time)
- **Hardware:** Old laptop, Raspberry Pi, school computer lab
- **Motivation:** "I want skills beyond my coursework. I want a portfolio."
- **Barriers:** Time, money, imposter syndrome, "I'm just a student"
- **What they need:** Quick wins, career relevance, community

### 2.2 The Career Shifter (Age 25-40)
- **Budget:** ₱15,000-₱40,000 (savings, deliberate investment)
- **Hardware:** Second-hand server, NUC, existing PC
- **Motivation:** "I need to prove I can do this job before anyone hires me"
- **Barriers:** Time pressure, knowledge gaps, "I'm behind everyone"
- **What they need:** Structured path, plain-language explanation, confidence

### 2.3 The Hobbyist (Age 20-55)
- **Budget:** ₱10,000-₱30,000 (fun money)
- **Hardware:** Whatever sounds interesting
- **Motivation:** "Building things is fun. I like making computers do cool stuff"
- **Barriers:** Overwhelm, "where do I start?", comparison to YouTubers
- **What they need:** Fun projects first, progressive complexity, encouragement

### 2.4 The Retiree / Late Bloomer (Age 50-70)
- **Budget:** ₱20,000-₱50,000 (disposable income)
- **Hardware:** Willing to buy quality, patient with learning
- **Motivation:** "I want to stay sharp. I want to understand the technology my kids use"
- **Barriers:** "I'm too old for this," steep learning curve, fast-paced tech world
- **What they need:** Slow pace, patient explanations, no jargon without explanation

### 2.5 The Parent (Age 30-50)
- **Budget:** ₱15,000-₱35,000 (family budget)
- **Hardware:** Wants something reliable and simple
- **Motivation:** "I want to control my family's data. I want to teach my kids about technology"
- **Barriers:** Reliability requirements, "if it breaks, the family suffers"
- **What they need:** Family-friendly projects, reliability focus, simplicity

---

## 3. Book Structure: The Journey

The book is organized as **two parts plus a capstone** in the current manuscript. Part III is intentionally left for future expansion, but it is not part of the published MVP. The progression should be readable both as a maturity ladder and as a walk through the house:

```
Current manuscript:
  Part I:  "Bahay Bahayan" (Home)  →  Consumer → Tinkerer   (Chapters 1-6)
  Part II: "Lumaki" (Grow)         →  Tinkerer → Builder     (Chapters 7-12)
          + Chapter 13 portfolio capstone

Future expansion:
  Part III: "Malaki" (Become)      →  Builder → Architect    (Not yet written)
```

**Current manuscript = 13 chapters** (Ch 1-12 + Ch 13 capstone). Each chapter is self-contained with working projects. Remote access is merged into Chapter 7 as Section 7.5.

The progression rules are simple:

- early chapters should feel like `Prep` and `Kinder` wins;
- Part II should steadily climb into `High School`, `College`, and `Masters` territory; and
- anything that feels like `PhD` or `Postdoc` belongs in deep dives, appendices, or future expansion unless it clearly supports the main story.

The household mapping should stay consistent:

- services that protect the home belong near power, backups, and security;
- services that help the household use the lab belong near productivity, media, and automation;
- services that manage the lab itself belong near Docker, Compose, proxies, and operations; and
- experimental services belong in `Malaki`, not the MVP core.

---

### Part I: "Bahay Bahayan" — Your First Steps (Chapters 1-6)

**Goal:** Get something running on a server in under 2 hours. Build confidence. Understand what a homelab is.

#### Chapter 1: "What Even Is a Homelab?"
**Type:** Conceptual / Motivational
**Reading Time:** 20 minutes

**What this chapter does:**
- Explains homelabbing in plain language (no jargon)
- Shows what's possible (pictures of real Filipino homelabs)
- Addresses common fears: "I'll break something," "It's too expensive," "I'm not technical enough"
- Introduces the values: ownership, privacy, learning, fun
- **Activity:** Pick your "why" — what do you want from your homelab?

**Key Message:** *"Your homelab doesn't need to be impressive. It needs to be yours."*

**Filipino Context:**
- Real stories from Filipino homelabbers
- Hardware available in SM Computer Center, Facebook Marketplace, Computer City
- ₱ prices, not $ or €
- PH internet provider realities (PLDT, Globe, DITO)

#### Chapter 2: "Your First Server"
**Type:** Hands-on / Hardware
**Time to Complete:** 30 minutes - 2 hours (depending on hardware)

**What this chapter does:**
- Hardware options from ₱0 (use what you have) to ₱50,000 (nice setup)
- **Path A: ₱0** — Use your existing laptop/PC
- **Path B: ₱5,000** — Raspberry Pi 4 or Orange Pi 5
- **Path C: ₱15,000** — Second-hand mini PC from FB Marketplace
- **Path D: ₱30,000+** — NUC or refurbished server
- Installing Ubuntu Server (step-by-step screenshots)
- Your first SSH connection
- **Activity:** Get a terminal prompt on a server

**Key Message:** *"You can start with literally ₱0. Your laptop counts."*

**Filipino Context:**
- FB Marketplace buying guide (what to look for, red flags)
- Computer City / SM Computer Center walkthrough
- Second-hand server communities in PH
- Power consumption calculator (₱/month electricity)

#### Chapter 3: "Hello, Container"
**Type:** Hands-on / Docker
**Time to Complete:** 1 hour

**What this chapter does:**
- What is Docker? (the "shipping container" analogy)
- Installing Docker
- Your first container: `docker run hello-world`
- Running a real service: a simple web server
- Docker Compose introduction
- **Activity:** Deploy your first self-hosted service (Uptime Kuma or similar)

**Key Message:** *"Containers are just really good at keeping things organized. That's it."*

**Filipino Context:**
- Download mirrors for slow internet
- What to do when Docker Hub is slow in the Philippines
- Mobile data considerations (some readers start on ₱200 load)

#### Chapter 4: "Something Useful"
**Type:** Project-based / First Real Service
**Time to Complete:** 2 hours

**What this chapter does:**
- Deploy something you'll actually use
- **Project options** (pick one):
  - **Vaultwarden** — your own password manager
  - **Jellyfin** — your own Netflix
  - **Nextcloud** — your own Google Drive
  - **Pi-hole** — block ads on your network
- Configuration and first use
- Accessing from your phone
- **Activity:** Use your service for a day

**Key Message:** *"A homelab that you don't use is just a paperweight. Build something you'll touch."*

**Filipino Context:**
- Which services replace expensive subscriptions (Netflix ₱199, Google One ₱59, etc.)
- Cost savings calculator: "This service costs ₱0. Netflix costs ₱199/month."
- Mobile access (most Filipinos are mobile-first)

#### Chapter 5: "Keeping It Alive"
**Type:** Operations / Reliability
**Time to Complete:** 1 hour

**What this chapter does:**
- What happens when your server restarts?
- Docker auto-start (recreate policy)
- Basic backup (copy your data somewhere)
- Health checks (is it still running?)
- Updates (keeping things safe)
- **Activity:** Set up automatic restart and a simple backup

**Key Message:** *"The difference between a toy and a tool is reliability."*

**Filipino Context:**
- Power outage considerations (brownout-proofing)
- Internet reliability (what happens when PLDT goes down)
- UPS recommendations for ₱2,000-₱5,000 budget

#### Chapter 6: "Your First Network"
**Type:** Networking / Foundation
**Time to Complete:** 2 hours

**What this chapter does:**
- What is an IP address? (plain language)
- Your home network explained (router, DHCP, ports)
- Making your services accessible on your network
- Static IP for your server
- Port forwarding (and why to be careful)
- **Activity:** Access your homelab from another device on your network

**Key Message:** *"Networking is just plumbing for the internet. You're the plumber now."*

**Filipino Context:**
- Common PH router models (PLDT, Globe, DITO)
- How to find your router admin page
- Port forwarding on common PH ISP routers

---

### Part II: "Lumaki" — Growing Your Lab (Chapters 7-13)

**Goal:** Build a multi-service homelab that you actually use daily. Learn the patterns that scale. Chapter 13 is the capstone portfolio chapter.

#### Chapter 7: "The Reverse Proxy + Remote Access"
**Type:** Hands-on / Networking
**Time to Complete:** 2 hours

**What this chapter does:**
- What is a reverse proxy? (the front desk analogy)
- Setting up Traefik or Caddy
- SSL certificates (Let's Encrypt, automatic)
- Accessing services by name: `vaultwarden.home.local`
- **Activity:** All your services accessible through one URL

**Filipino Context:**
- What to do if you don't have a domain (free options)
- Accessing from outside your home (and the security considerations)

#### Chapter 8: "Multiple Services, One System"
**Type:** Architecture / Organization
**Time to Complete:** 3 hours

**What this chapter does:**
- Organizing Docker Compose files
- Shared networks and volumes
- Environment variables and configuration
- Your first "stack"
- **Project:** Deploy a complete productivity stack (Nextcloud + Vaultwarden + Uptime Kuma, all together)
- **Activity:** Add a new service in 10 minutes

**Filipino Context:**
- Storage considerations (₱ prices for SSDs, HDDs)
- Where to buy: Lazada, Shopee, Computer City

#### Chapter 9: "Your Data, Safe"
**Type:** Backup / Disaster Recovery
**Time to Complete:** 2 hours

**What this chapter does:**
- The 3-2-1 backup rule (explained simply)
- Automated backups with Restic or Borg
- Testing restores (the most important part)
- Offsite backup (Google Drive, Backblaze, or a friend's house)
- **Activity:** Backup your data and restore it

**Filipino Context:**
- ₱0 backup options (external USB drive, friend's house)
- ₱500/month backup options (Backblaze)
- Flood/fire consideration (common in PH)

#### Chapter 10: "Automation: Work Less"
**Type:** Scripting / Automation
**Time to Complete:** 2-3 hours

**What this chapter does:**
- Why automate? (you'll forget, so let the computer remember)
- Basic Bash scripting
- Cron jobs (scheduled tasks)
- Updating containers automatically (Watchtower)
- **Project:** Write scripts for common tasks
- **Activity:** Set up automatic updates

**Filipino Context:**
- What to automate first (time-savers for busy Filipinos)
- Work-life balance: automation frees up time for family

#### Chapter 11: "Monitoring: Eyes on Everything"
**Type:** Observability / Operations
**Time to Complete:** 2 hours

**What this chapter does:**
- Why monitor? (you can't fix what you don't know is broken)
- Uptime Kuma (simple) vs. Prometheus + Grafana (advanced)
- Setting up alerts (get notified when things break)
- Dashboard creation
- **Project:** Build a monitoring dashboard
- **Activity:** Get a notification when a service goes down

**Filipino Context:**
- Mobile notifications (Telegram bot for alerts)
- Data-saver monitoring (not all Filipinos have unlimited data)

#### Chapter 12: "Security: Don't Get Hacked"
**Type:** Security / Hardening
**Time to Complete:** 2 hours

**What this chapter does:**
- Common mistakes that expose your homelab
- Firewall basics (UFW)
- SSH key authentication (no more passwords)
- Fail2ban (block bad actors)
- When NOT to expose services to the internet
- WireGuard VPN for remote access
- **Project:** Harden your entire homelab
- **Activity:** Test your security with a simple scan

**Filipino Context:**
- PH-specific threats (what hackers target in the Philippines)
- Data privacy law context (Philippine Data Privacy Act of 2012)
- Affordable security options

#### Chapter 13: "Your Homelab Portfolio" (MVP Capstone)
**Type:** Career / Documentation
**Time to Complete:** 2 hours

**What this chapter does:**
- Documenting what you built
- Architecture diagrams (simple tools)
- Writing your homelab story
- Optional career translation: "I run Jellyfin" → "I maintain a self-hosted media streaming platform"
- Building a portfolio repository
- **Project:** Create your homelab portfolio
- **Activity:** Write a "My Homelab" document you can share

**Filipino Context:**
- Philippine job market context
- Remote work opportunities (₱ vs $ salaries)
- How to talk about homelab in PH job interviews

---

**Current manuscript = 13 chapters:** Ch 1-6 (Part I) + Ch 7-13 (Part II + capstone). Remote Access is merged into Ch 7 as Section 7.5.

---

### Part III: "Malaki" — Becoming an Architect (Chapters 14-23) — **v2 / Future**

> ⚠️ **NOT in MVP.** Part III is planned for v2 after Parts I + II are published and community feedback is gathered.

**Goal:** Design systems, not just deploy services. Choose your specialization path. This is where the book can go deeper into game hosting, Dokploy-style service orchestration, and local hosted LLMs without bloating the core manuscript.

#### Chapter 14: "Choose Your Path"
**Type:** Conceptual / Roadmap
**Reading Time:** 30 minutes

**What this chapter does:**
- Overview of specialization paths
- How to choose based on your goals
- Paths:
  - **DevOps** → Kubernetes, CI/CD, GitOps
  - **Security** → SIEM, penetration testing, identity
  - **AI/ML** → Local LLMs, RAG, inference
  - **Media** → Advanced streaming, automation, DVR
  - **Privacy** → Self-hosted everything, encryption
  - **IoT** → Home automation, sensors, energy monitoring
  - **Games** → Hosted game servers, latency, player access, recovery
  - **AI** → Local hosted LLMs, RAG, private assistants
  - **Operations** → Dokploy-style service control, deploy flows, lifecycle management
- **Activity:** Pick your path and set goals

**Key Message:** *"You don't need to learn everything. Pick what excites you."*

#### Chapter 15: "Kubernetes for Real Humans"
**Type:** Advanced / DevOps
**Time to Complete:** 4-6 hours

**What this chapter does:**
- What is Kubernetes? (the "orchestra conductor" analogy)
- k3s (lightweight Kubernetes for homelabs)
- Your first deployment
- Services, Ingress, Persistent Volumes
- When Kubernetes makes sense (and when it doesn't)
- **Project:** Deploy your services on k3s
- **Activity:** Scale a service with one command

**Filipino Context:**
- Is Kubernetes worth it for your homelab? (honest answer)
- When to skip Kubernetes and use Docker Compose

#### Chapter 16: "Local AI: Your Own Intelligence"
**Type:** Advanced / AI
**Time to Complete:** 3-4 hours

**What this chapter does:**
- What is a local LLM?
- Hardware requirements (what GPU do you need?)
- Installing Ollama
- Running your first model
- Building a simple RAG system
- **Project:** Chat with your documents
- **Activity:** Ask your homelab questions about your files

**Filipino Context:**
- GPU availability in the Philippines (prices, where to buy)
- ₱5,000 GPU options (used GTX 1060)
- Running AI without a GPU (CPU inference, small models)
- Tagalog language model options

#### Chapter 17: "Game Room: Hosting What the House Uses"
**Type:** Advanced / Household Use Case
**Time to Complete:** 3-4 hours

**What this chapter does:**
- Hosting game servers for family and friends
- Why some games need direct UDP/TCP access
- CPU, RAM, storage, and latency tradeoffs
- Recovery after crashes or updates
- **Project:** Run one small private game server
- **Activity:** Let someone else connect to it successfully

#### Chapter 18: "The Operations Desk"
**Type:** Advanced / Service Control Plane
**Time to Complete:** 3 hours

**What this chapter does:**
- Managing services through a control plane instead of one-off commands
- Comparing raw Docker Compose, UI-first managers, and platform-style tools
- Environment variables, deploys, rollbacks, and logs
- When a tool like Dokploy fits a homelab
- **Project:** Move one service under managed deployment
- **Activity:** Redeploy, update, and recover a service cleanly

#### Chapter 19: "Home Automation"
**Type:** Advanced / IoT
**Time to Complete:** 3-4 hours

**What this chapter does:**
- Home Assistant installation
- Connecting your first device
- Automations and scenes
- Dashboard creation
- **Project:** Automate something in your home
- **Activity:** Control a device from your phone

**Filipino Context:**
- Smart home devices available in PH (Shopee, Lazada)
- ₱500 smart plug automation
- Power monitoring (track your electricity bill)

#### Chapter 20: "High Availability"
**Type:** Advanced / Reliability
**Time to Complete:** 4-6 hours

**What this chapter does:**
- What is high availability?
- Redundancy patterns
- Failover testing
- Multi-server setup
- **Project:** Build a two-server failover
- **Activity:** Pull the plug and watch your lab survive

**Filipino Context:**
- Is HA worth it for your homelab? (honest cost/benefit)
- ₱0 HA options (software redundancy)

#### Chapter 21: "Infrastructure as Code"
**Type:** Advanced / Professional
**Time to Complete:** 3-4 hours

**What this chapter does:**
- What is IaC? (the "recipe book" analogy)
- Ansible basics
- Documenting your infrastructure
- Reproducing your homelab from scratch
- **Project:** Write Ansible playbooks for your setup
- **Activity:** Rebuild your homelab from code

**Filipino Context:**
- Why this matters for PH job market
- Career relevance of IaC skills

#### Chapter 22: "Community & Contribution"
**Type:** Community / Growth
**Reading Time:** 30 minutes

**What this chapter does:**
- Finding your homelab community
- Contributing to open-source projects
- Writing tutorials (what you learned, help others)
- Mentoring beginners
- **Activity:** Join a community and introduce yourself
- **Activity:** Contribute to a project you use

**Filipino Context:**
- Filipino homelab communities (Facebook groups, Discord)
- Local meetups (Manila, Cebu, Davao)
- How to contribute when English isn't your first language

#### Chapter 23: "What's Next?"
**Type:** Vision / Future
**Reading Time:** 20 minutes

**What this chapter does:**
- Where homelabbing is heading
- Personal cloud vision
- Sovereign AI
- Your homelab as your digital life OS
- Continuing education paths
- **Activity:** Set goals for the next year

**Key Message:** *"You're not done. You're just getting started."*

---

## 4. Pedagogical Design

### 4.1 Learning Approach: "Just Enough Theory, Then Build"

Each chapter follows this pattern:

```
Story/Analogy (2-3 min read)
→ "Why this matters" (1 min)
→ "What you'll build" (1 min)
→ Hands-on steps (15-60 min)
→ "What you learned" (2 min)
→ "What to do next" (1 min)
→ "Go deeper" (optional links)
```

**The 80/20 Rule:** 80% hands-on, 20% theory. If a concept can be learned through doing, we do it.

### 4.2 Progressive Disclosure

The book uses **three layers** for every topic:

**Layer 1: The Quick Start** (everyone reads this)
- Minimal explanation
- Copy-paste commands
- Working result in < 30 minutes

**Layer 2: The "Why"** (readers who want to understand)
- How the technology works
- Why we chose this approach
- What alternatives exist

**Layer 3: The Deep Dive** (optional, for the curious)
- Architecture diagrams
- Protocol details
- Performance considerations
- Security implications

**Visual Indicator:**
- 🟢 Green box = Quick Start (required)
- 🔵 Blue box = The Why (recommended)
- 🟣 Purple box = Deep Dive (optional)

### 4.3 Failure-Friendly Design

Based on research showing that failure-based learning creates deeper understanding:

**"Expected Failures" Section** in each chapter:
- "You might see this error. Here's why it happens and how to fix it."
- "If X doesn't work, try Y instead."
- "This is normal. Don't panic."

**"Break It" Exercises:**
- Intentionally break things to learn recovery
- "Delete this file. Now fix it."
- "Stop this container. Watch what happens."

### 4.4 Filipino Context Integration

Every chapter includes Filipino-specific content:

**Hardware:**
- ₱ prices with current market rates
- Where to buy: FB Marketplace, Computer City, Shopee, Lazada
- Second-hand buying guide
- Power consumption in ₱/month (based on Meralco rates)

**Internet:**
- PH ISP considerations (PLDT, Globe, DITO, Converge)
- Upload speed limitations
- Public IP availability
- CGNAT workarounds (Cloudflare Tunnel)

**Cultural:**
- Taglish examples where natural
- Local analogies (jeepney for networking, sari-sari store for services)
- Filipino homelabber stories
- Family context (apartment living, shared spaces)

**Economic:**
- Cost comparison: self-hosted vs. SaaS subscriptions in ₱
- Return on investment calculations
- Career salary data (PH market)

---

## 4.5 Taglish Writing Style Guide

**Core Principle:** Write naturally. Don't force Filipino where English is more natural, and don't force English where Filipino feels right.

**Rules:**
- **Tech terms stay in English:** Docker, container, server, IP address, firewall, backup, API, database
- **Narration in Taglish:** "Okay, let's set up our server. First, i-install natin ang Docker."
- **Instructions in clear English:** Code blocks and commands always in English
- **Explanations in Taglish:** "Ang reverse proxy is like the front desk ng bahay..."
- **Jargon alerts in Filipino:** Explain technical terms with Filipino analogies
- **Tone:** Conversational, like explaining to a friend over coffee
- **Avoid:** Over-formal Tagalog, excessive slang, code-switching mid-sentence for style

**Examples:**
- ✅ "I-start natin ang container. I-type mo ito sa terminal:"
- ✅ "Ang Docker image is like a template. Parang cookie cutter — i-copy mo lang kung gaano karaming cookies kailangan mo."
- ❌ "Ipaganda natin ang aesthetics ng interface" (unnatural mixing)
- ❌ "Gusto kitang i-instruct na i-execute ang command" (too formal)

**Code & Commands:**
- Always English
- Comments can be Taglish: `# i-update natin ang system`
- Filenames in English

**PH-Specific Content:**
- Use ₱ symbol, not PHP or pesos
- Reference local places naturally: "Kung sa Computer City ka..."
- ISP names: PLDT, Globe, DITO, Converge
- Power: Meralco rates, brownout context

### 5.1 Diagram Style

**Architecture diagrams** use simple, consistent icons:
- Server = box with ₱ price range
- Service = icon with name
- Connection = arrow with protocol label
- Internet = cloud icon

**Network diagrams** show:
- Router (with common PH ISP label)
- Devices (phone, laptop, server)
- Data flow (colored arrows)

### 5.2 Code Presentation

```bash
# This is your first command
# It will ask for your password
sudo apt update
```

Every code block includes:
- Filename/context comment
- What the command does (plain language)
- Expected output
- What to do if it fails

### 5.3 Callout System

Consistent with `agents.md`:

> **💡 Quick Win:** You can do this in 5 minutes. Here's how.

> **⚠️ Watch Out:** Common mistake that wastes hours. Read this first.

> **💸 Lean Path:** How to do this for ₱0 or with what you have.

> **🚀 Turbo:** If you have more hardware, here's how to go faster.

> **🧠 Deep Dive:** For the curious. Skip if you just want it working.

> **📢 Jargon Alert:** Technical term explained in plain language.

> **💼 Career Boost:** Optional career value when it helps the reader.

> **🇵🇭 PH Context:** Philippines-specific consideration.

---

## 6. Content Delivery & Format

### 6.1 Primary Format: Web-First

**Why web-first:**
- Free to access (no PDF download needed)
- Searchable
- Updatable
- Mobile-friendly (most Filipinos read on phones)
- Can embed interactive elements

**Platform:** Static site generator (Hugo or MkDocs) hosted on GitHub Pages (free)

### 6.2 Secondary Format: PDF

- Generated from web version
- Available for offline reading
- Print-friendly layout

### 6.3 Interactive Elements

**Terminal Simulator:**
- In-browser terminal for practicing commands
- No server needed

**Architecture Builder:**
- Drag-and-drop diagram tool
- Save and share your setup

**Progress Tracker:**
- Check off completed chapters
- See your homelab maturity level
- Share progress with community

### 6.4 Video Companions (Optional)

- YouTube walkthroughs for complex chapters
- Filipino creators preferred
- Subtitled in Filipino languages

---

## 7. Fun Elements & Engagement

### 7.1 The Homelab RPG (Simplified)

Based on our research's gamification concept, but simplified for beginners:

**10 Levels:**
| Level | Name | What You've Done |
|-------|------|-----------------|
| 1 | Bahay | Built your first service |
| 2 | Dumaan | Set up networking |
| 3 | Naka-backup | Automated backups |
| 4 | Naka-monitor | Monitoring dashboard |
| 5 | Naka-secure | Hardened your lab |
| 6 | Naka-virtualize | Running VMs |
| 7 | Naka-automate | Scripts doing your work |
| 8 | Naka-scale | Multiple servers |
| 9 | Naka-AI | Running local AI |
| 10 | Arkitekto | Full homelab architect |

**Achievement Badges:**
- 🏅 **First Blood:** Your first service deployed
- 💸 **Sulit Master:** Full stack for under ₱10,000
- 🔥 **Brownout Survivor:** Lab survived a power outage
- 📚 **Taga-Turo:** Helped another beginner
- 🐛 **Bug Hunter:** Found a bug in a project you use

### 7.2 "Show Your Lab" Community

- Share photos of your setup
- Monthly challenges ("Build X in 48 hours")
- Filipino homelab gallery
- "Setup of the Month" feature

### 7.3 Real Filipino Homelab Stories

Each section includes a **"Meet a Filipino Homelabber"** sidebar:

**Example:**
> **Juan, 34, BPO worker from Cebu**
> *"I started with a ₱3,000 Raspberry Pi because I wanted something to do after my night shift. Now I'm learning DevOps and applying for cloud jobs. My homelab got me my first interview."*
> Setup: Raspberry Pi 4, ₱8,000 total, 5 services running

### 7.4 "What You Can Cancel" Calculator

Interactive tool showing:
- List of common SaaS subscriptions (Netflix, Google One, Dropbox, etc.)
- Monthly cost in ₱
- Self-hosted alternative
- Annual savings

**Example:**
| Service | Monthly Cost | Self-Hosted Alternative | Annual Savings |
|---------|-------------|------------------------|----------------|
| Netflix ₱199 | ₱2,388/yr | Jellyfin | ₱2,388 |
| Google One 100GB ₱59 | ₱708/yr | Nextcloud | ₱708 |
| LastPass ₱2 | ₱24/yr | Vaultwarden | ₱24 |
| **Total** | **₱264/mo** | | **₱3,120/yr** |

---

## 8. Chapter Template

Every chapter follows this exact structure:

```markdown
# Chapter X: [Title]

## What You'll Build
[2-3 sentences describing the end result]

## How Long It Takes
[X hours] (including reading)

## What You Need
- [hardware requirement]
- [software prerequisites]
- [previous chapters needed]

## The Story
[2-3 paragraph analogy or story that makes this relatable]

## Why This Matters
[1 paragraph on why this skill is valuable]

---

## 🟢 Quick Start
[Step-by-step instructions for getting something working]

### Step 1: [Action]
[Instructions with code blocks]

**Expected output:**
```
[What they should see]
```

**If you see an error:**
> ⚠️ [Common error and fix]

[Repeat for each step]

---

## 🔵 The Why
[How this technology works]
[Why we chose this approach]
[Alternatives and trade-offs]

---

## 🟣 Deep Dive
[Architecture diagrams]
[Protocol details]
[Performance considerations]

---

## 💼 Career Boost
[How to describe this clearly when needed]
[Interview talking points]
[Related job roles in the Philippines]

---

## 🇵🇭 PH Context
[Local hardware where to buy]
[PH internet considerations]
[Cost in ₱]

---

## What's Next
[What chapter to read next]
[Optional projects]

## Go Deeper
[Links to documentation, videos, communities]
```

---

## 9. Appendix

### Appendix A: Glossary
- 100+ terms explained in plain Filipino English
- Tagalog equivalents where possible
- "If you know X, it's like Y" comparisons

### Appendix B: Hardware Buying Guide
- Where to buy in the Philippines
- FB Marketplace red flags
- Computer City walkthrough
- Current prices (updated quarterly)
- Power consumption calculator

### Appendix C: Common Errors & Fixes
- 50 most common errors
- Searchable by error message
- Plain-language explanations

### Appendix D: Service Directory
- 50 services organized by category
- Resource requirements
- Difficulty rating
- "Best for beginners" tag

### Appendix E: Community Resources
- Filipino homelab communities
- Facebook groups
- Discord servers
- YouTube channels
- Local meetups

### Appendix F: Commands Reference
- Common Linux commands
- Docker commands
- Network commands
- Quick lookup format

---

## 10. Development Plan (MVP-First)

### Phase 0: Foundation (Weeks 1-2)
- Finalize book name and branding
- Set up GitHub repository with `agents.md`
- Build web platform (Hugo/MkDocs on GitHub Pages)
- Create chapter template and visual identity
- Write Taglish style guide (what stays English, what goes Filipino)
- **Milestone:** Repo live, template ready

### Phase 1: Part I — "Bahay Bahayan" (Weeks 3-8)
- Write Chapters 1-6 in Taglish
- Each chapter: Quick Start → The Why → Deep Dive
- Filipino homelabber stories collection
- Beta reader feedback loop
- **Milestone:** Part I published (6 chapters)

### Phase 2: Part II — "Lumaki" (Weeks 9-16)
- Write Chapters 7-12 in Taglish
- Interactive elements (progress tracker, cost calculator)
- Community launch (Discord/Facebook group)
- **Milestone:** MVP complete (12 chapters, all of Parts I + II)

### Phase 3: Polish & Community (Weeks 17-20)
- Appendix content (glossary, hardware guide, errors)
- Community contributions workflow
- Translation pipeline setup
- **Milestone:** MVP v1.0 with community

### Phase 4: Part III — "Malaki" (Future, v2)
- Chapters 14-23 (Kubernetes, AI, game hosting, service control, HA, IaC, etc.)
- Specialization paths
- Video companions
- **Milestone:** Full book v2.0

> **MVP Philosophy:** Ship the 12-chapter core first. Get real readers. Iterate based on feedback. Part III comes when the community is ready.

---

## 11. Success Metrics

### MVP Outcomes (Weeks 1-16):
- 13 chapters written and published (Ch 1-12 + Ch 13 capstone, Remote Access merged into Ch 7)
- 100+ GitHub stars
- 50+ community members (Discord/Facebook)
- 10+ community contributions (PRs, translations, stories)
- Beta reader feedback incorporated

### Reader Outcomes (90 days after reading):
- 80% have deployed at least one service
- 50% have a working multi-service homelab
- 30% have joined a homelab community
- 20% have contributed to open-source
- 10% have used homelab skills in their career

### Book Metrics (Ongoing):
- Stars/forks on GitHub
- Community growth (Discord/Facebook)
- Contribution rate (PRs, translations)
- Citation by other resources
- Job placement stories collected
- Monthly active readers

---

## 12. Open Questions

1. **Name:** ✅ Decided — "Bahay Bahayan: Build Your First Homelab"
2. **Format:** Static site vs. interactive platform vs. both?
3. **Community:** Build alongside the book, or launch after?
4. **Updates:** How to keep hardware prices and software versions current?
5. **Validation:** Beta readers from target personas before full publication?
6. **Part III:** When to expand beyond the current 13-chapter manuscript?

### Decided
- ✅ **Language:** Taglish (natural Filipino + English mix, tech terms in English)
- ✅ **Scope:** Current manuscript first — 13 chapters (Parts I + II + capstone), Part III deferred to future expansion
- ✅ **License:** Open source, free forever (CC BY-SA 4.0), no paid tier

---

## 13. Inspiration & References

This design draws from:

- **Our research:** `research/homelab-ecosystems-research.md` — all 10 dimensions
- **Our ideas:** `ideas.md` — journey phases, growth, values
- **Our manifesto:** `agents.md` — A.C.E.S. structure, voice, tone
- **Our profiles:** `professional-profiles-matrix.md` — 14 profiles, service catalog
- **Home Assistant:** Community-first, beginner-friendly, documentation quality
- **Awesome Selfhosted:** 293k stars, 100+ categories, community-curated
- **Philippine context:** Local prices, hardware availability, cultural references

---

*This design document is a living document. It will evolve as we write, receive feedback, and learn from our readers.*

**Version:** 1.0
**Date:** May 2026
**Status:** Draft — ready for feedback
