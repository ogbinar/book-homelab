# Chapter 14: Your Homelab Portfolio

## What You'll Build
A professional portfolio document that shows what you've built, translates your homelab skills into career language, and gives you something to share with employers, friends, or the homelab community. By the end, you'll have a living document that grows with your lab.

## How Long It Takes
2 hours (including reading, writing, and setting up a GitHub repo)

## What You Need
- Your homelab stack running (Chapters 1-12)
- A GitHub account (free)
- Screenshots of your services running
- Your "why" from Chapter 1

---

## The Story

You've spent weeks (or months) building your homelab. You've deployed services, configured networks, set up backups, and hardened your security. You feel proud.

But here's the problem: **no one else knows.**

Unless you tell someone, your homelab is invisible. And in the professional world, invisible work doesn't count — no matter how much you learned or how much it helped you grow.

This chapter is about making your work visible. Not bragging. Not showing off. But **documenting what you built** so that:
- Employers see your skills in action
- Friends and family understand what you do
- Your future self remembers what you figured out
- Other beginners can learn from your journey

Think of your portfolio as a **bridge between what you know and what you can show.**

> **📢 Jargon Alert:** "Portfolio" — A collection of your best work, organized to demonstrate your skills. In tech, a portfolio is often more valuable than a resume because it shows proof, not just claims.

---

## Why This Matters

### The "Experience Paradox"

Here's a frustrating truth about the job market:
- Entry-level jobs require "experience"
- Experience requires a job
- You can't get the job without the experience

It's a catch-22. Unless you know about homelabs.

**A homelab IS experience.** It's hands-on work with the same tools, concepts, and challenges that professionals use every day. The only difference is that you're doing it at home instead of in an office.

When an employer asks "Do you have experience with Docker?" you can say:

> *"Yes. I designed and maintain a multi-service homelab using Docker and Docker Compose, running 7 services including a password manager, file storage, and ad blocker. The infrastructure includes automated backups, monitoring, and security hardening."*

That's not "homelab experience." That's **real infrastructure experience.** The only difference is the context.

### Why Document?

1. **For your career:** Recruiters and hiring managers can see your work
2. **For your learning:** Writing about what you built forces you to understand it deeply
3. **For your community:** Other beginners will learn from your journey
4. **For your future self:** You'll forget what you did. Documentation saves you from that.

---

## 🟢 Quick Start

### Step 1: Write Your "Homelab Story"

Open a document (Google Docs, Notion, or a Markdown file). Answer these questions:

1. **What is your homelab?** One-paragraph description of what it does
2. **Why did you start?** Your "why" from Chapter 1
3. **What services are running?** List them with brief descriptions
4. **What hardware do you use?** Server type, RAM, storage, budget
5. **What challenges did you face?** The problems you solved
6. **What are you most proud of?** The achievement you want to highlight
7. **What's next?** Your goals for the future

**Example:**

> **My Homelab**
>
> I run a single-server homelab on a second-hand Dell OptiPlex Micro (i5-6500, 8GB RAM, 256GB SSD) for ₱10,000. It runs 6 Docker containers: Uptime Kuma (monitoring), Vaultwarden (password manager), Nextcloud (file storage), Pi-hole (ad blocking), Jellyfin (media server), and Caddy (reverse proxy).
>
> I started because I was tired of paying ₱300/month for subscriptions I barely used. My homelab has saved me ₱3,600/year and taught me real infrastructure skills.
>
> Biggest challenge: Getting Pi-hole to work with my PLDT router. Took 3 hours of DNS debugging. Learned more in those 3 hours than in any online course.
>
> Next goal: Add a second server for redundancy and learn Kubernetes.

### Step 2: Take Screenshots

For each service, take a screenshot showing:
- The dashboard or main interface
- The service name and URL
- Any interesting stats (uptime, storage used, etc.)

You'll want:
- Uptime Kuma dashboard
- At least one service dashboard (Vaultwarden, Nextcloud, etc.)
- Your server's system info (`docker ps` or `docker system df`)
- Any custom configuration you've done

**Pro tip:** Use a clean browser window. Close unnecessary tabs. Make it look professional.

### Step 3: Create an Architecture Diagram

You don't need fancy tools. A simple text diagram works:

```
┌──────────────────────────────────────────────┐
│              YOUR SERVER                     │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Caddy   │  │  Docker  │  │  Pi-hole │   │
│  │  (Proxy) │  │(Services)│  │  (DNS)   │   │
│  └────┬─────┘  └────┬─────┘  └──────────┘   │
│       │             │                        │
│       └──────┬──────┘                        │
│              ▼                               │
│      ┌──────────────┐                        │
│      │  Uptime Kuma │                        │
│      │  (Monitor)   │                        │
│      └──────────────┘                        │
└──────────────────────────────────────────────┘
         │
         ▼
┌──────────────────┐    ┌──────────────────┐
│  Your Phone      │    │  Your Laptop     │
│  (Access)        │    │  (Access)        │
└──────────────────┘    └──────────────────┘
```

**Tools for better diagrams:**
- [Excalidraw](https://excalidraw.com/) — Free, hand-drawn style, browser-based
- [Draw.io](https://app.diagrams.net/) — Free, professional diagrams
- Even a screenshot of a hand-drawn diagram works fine

### Step 4: Create a GitHub Repository

This is where your portfolio lives.

```bash
# Create a new repository on GitHub
# Go to github.com/new and create:
# - Name: homelab
# - Description: My homelab setup and documentation
# - Public (so employers can see it)
# - Add a README.md

# Clone it locally
git clone https://github.com/yourusername/homelab.git
cd homelab

# Add your documentation
echo "# My Homelab" > README.md
```

**Your README should look like this:**

```markdown
# My Homelab

> Started: [Date]
> Server: [Hardware description]
> Budget: ₱[Amount]
> Services: [Count] running

## Overview

[Your homelab story — 2-3 paragraphs]

## Services

| Service | Purpose | URL |
|---------|---------|-----|
| Uptime Kuma | Monitoring | kuma.home.local |
| Vaultwarden | Password Manager | vault.home.local |
| Nextcloud | File Storage | nextcloud.home.local |
| Pi-hole | Ad Blocking | pihole.home.local |
| Jellyfin | Media Server | jellyfin.home.local |
| Caddy | Reverse Proxy | proxy.home.local |

## Hardware

- **Server:** [Model, CPU, RAM, Storage]
- **Budget:** ₱[Amount]
- **Power:** ~[W] idle, ~₱[Amount]/month electricity

## Architecture

![Architecture Diagram](diagrams/architecture.png)

## What I've Learned

- Docker and container orchestration
- Network configuration and reverse proxies
- Backup strategies and disaster recovery
- Security hardening (SSH keys, firewalls)
- Automation with cron and scripts
- Monitoring and observability

## What I'm Working On

- [Next goal 1]
- [Next goal 2]

## License

Documentation is shared under [your choice of license].
Code configurations are shared under MIT License.
```

### Step 5: CV Translation — From "Homelab" to "Professional"

This is the most important step. You need to translate your homelab work into language that HR and hiring managers understand.

**The translation table:**

| Homelab Term | Professional Term |
|---|---|
| "I run a homelab" | "I manage self-hosted infrastructure" |
| "I installed Docker" | "I deployed containerized applications" |
| "I set up Pi-hole" | "I implemented network-level content filtering" |
| "I configured Caddy" | "I deployed a reverse proxy with automated TLS" |
| "I set up backups" | "I designed a disaster recovery strategy" |
| "I use cron jobs" | "I automate infrastructure maintenance" |
| "I monitor with Grafana" | "I implement observability and alerting" |

**Before and After examples:**

> ❌ **Bad:** "I have a homelab with Docker and some services."

> ✅ **Good:** "I designed and maintain a self-hosted infrastructure stack running 6+ services including monitoring (Prometheus/Grafana), security (Vaultwarden), and file storage (Nextcloud). Infrastructure is managed via Docker Compose with automated backups and health monitoring."

> ❌ **Bad:** "I learned networking by setting up my homelab."

> ✅ **Good:** "I implemented network architecture including static IP assignment, DNS configuration (Pi-hole), reverse proxy routing (Caddy), and SSL/TLS management for internal services."

**Interview talking points:**

1. **Infrastructure:** *"I manage a production-like environment running 6+ services using Docker and Docker Compose. The stack includes automated backups, monitoring, and security hardening."*

2. **Problem-solving:** *"I debugged a DNS resolution issue with Pi-hole by analyzing router configuration and container networking. This taught me about subnetting, DHCP, and DNS propagation."*

3. **Automation:** *"I automated infrastructure maintenance using cron jobs and Watchtower, reducing manual maintenance from 4 hours/week to 30 minutes/week."*

4. **Security:** *"I implemented security best practices including SSH key authentication, firewall configuration (UFW), container isolation, and automated security updates."*

5. **Reliability:** *"I designed a 3-2-1 backup strategy with automated daily backups, monthly restore verification, and offsite redundancy."*

---

## 🔵 The Why

### Writing About What You Built Teaches You More

There's a teaching method called the [Feynman Technique](https://www.feynman-technique.com/) — when you try to explain a concept simply, you discover what you don't understand.

When you write about your homelab:
- You realize you don't actually understand *why* Pi-hole works the way it does
- You discover you've never tested your backups properly
- You find gaps in your knowledge that you can now fill

**Documentation is learning.** Every time you write about something, you understand it better.

### The Filipino Homelab Community

The Filipino homelab community is growing. People like:
- **Techno Tim** (YouTube) — Homelab setup videos
- **NetworkChuck** (YouTube) — Networking and homelab content
- **Linus Tech Tips** — Mainstream tech content with homelab segments
- **Local FB groups** — "Homelab Philippines," "Self-Hosted Philippines"

When you share your journey, you're contributing to a community that helps others. Your struggles become someone else's shortcuts.

---

## 🟣 Deep Dive

### Portfolio Structure Template

Use this structure for your portfolio README:

```markdown
# [Your Name]'s Homelab

## Quick Stats
- **Started:** [Date]
- **Server:** [Hardware]
- **Budget:** ₱[Amount]
- **Uptime:** [X] days (since last reboot)
- **Services:** [Count] running
- **Power:** ~[W] idle

## Architecture
[Diagram or link to diagram]

## Services
[Table with service, purpose, URL, status]

## Infrastructure Stack
- **OS:** Ubuntu Server 24.04 LTS
- **Container Runtime:** Docker + Docker Compose
- **Reverse Proxy:** Caddy
- **DNS:** Pi-hole
- **Monitoring:** Prometheus + Grafana + Uptime Kuma
- **Backup:** rsync + Restic (optional)
- **Security:** SSH keys, UFW, fail2ban

## Project History
- **[Date]:** Deployed first service (Uptime Kuma)
- **[Date]:** Added Vaultwarden for password management
- **[Date]:** Set up reverse proxy for clean URLs
- **[Date]:** Implemented backup strategy
- **[Date]:** Added monitoring (Prometheus/Grafana)

## Lessons Learned
1. [Lesson 1]
2. [Lesson 2]
3. [Lesson 3]

## What's Next
- [ ] [Next goal 1]
- [ ] [Next goal 2]
- [ ] [Next goal 3]

## Links
- GitHub: github.com/yourusername/homelab
- Portfolio: [Your website or Notion page]
- LinkedIn: linkedin.com/in/yourprofile
```

### Sharing Your Portfolio

Where to share your work:

1. **GitHub** — The standard for tech portfolios. Employers check it.
2. **LinkedIn** — Post about your homelab. Tag it #homelab #selfhosted. Filipino tech recruiters are active here.
3. **Homelab Forum** — Share your setup. Get feedback. Learn from others.
4. **Personal website** — Once you build one (Chapter 13 in v2), link to it.
5. **Facebook groups** — "Homelab Philippines," "Self-Hosted Philippines"

> **💡 Quick Win:** Post your homelab story on LinkedIn this week. Use the professional translations above. You might get a recruiter reaching out.

---

## 💼 Career Boost

### How to Talk About Your Homelab in Interviews

When asked "Tell me about yourself" or "What projects have you worked on?":

> *"Outside of work, I design and maintain a self-hosted infrastructure stack. I run 6 services using Docker and Docker Compose — including a password manager, file storage, ad blocker, and monitoring system. The infrastructure includes automated backups, security hardening, and observability with Prometheus and Grafana. I started it to save on subscriptions, but it became the fastest way I've found to learn real infrastructure skills. The biggest challenge was getting Pi-hole working with my router — it taught me more about DNS and networking than any course could."*

### CV/Resume Entries

Add this section to your resume:

```
Self-Hosted Infrastructure | Personal Project | [Date] - Present
• Designed and maintain a 6-service homelab stack using Docker and Docker Compose
• Implemented automated backups (3-2-1 strategy) with monthly restore verification drills
• Deployed monitoring and alerting stack (Prometheus, Grafana, node-exporter)
• Configured reverse proxy (Caddy) with automatic TLS for internal services
• Implemented security hardening: SSH key authentication, firewall (UFW), fail2ban
• Automated infrastructure maintenance with cron jobs and Watchtower, reducing
  manual maintenance from 4 hours/week to 30 minutes/week
• Total cost: ₱10,000 one-time (hardware) + ~₱200/month (electricity)
```

### Remote Work Opportunities

Homelab skills translate directly to remote work roles:
- **DevOps Engineer** — Docker, automation, monitoring
- **Site Reliability Engineer** — Uptime, reliability, backups
- **Cloud Engineer** — Same concepts, just in the cloud
- **Systems Administrator** — Linux, networking, security
- **Backend Engineer** — Understanding infrastructure makes you a better developer

**Filipino remote jobs that value homelab experience:**
- Remote DevOps roles (US/EU companies hiring in PH) — ₱100,000-₱300,000/month
- Remote SRE roles — ₱120,000-₱350,000/month
- Remote backend engineering — ₱80,000-₱200,000/month
- Local PH tech companies with infrastructure roles — ₱50,000-₱150,000/month

---

## 🇵🇭 PH Context

### Philippine Job Market: Homelab vs. Certifications

In the PH job market, you have two options to prove your skills:

| Option | Cost | Time | Value |
|---|---|---|---|
| **Homelab portfolio** | ₱0-₱15,000 (hardware) | Months | **Very high** — shows real work |
| **AWS/Azure/GCP cert** | ₱3,000-₱10,000 (exam) | 1-3 months | **High** — but theoretical |
| **Both** | ₱5,000-₱20,000 | 3-6 months | **Maximum** — proof + credential |

**The best strategy:** Build a homelab (free/cheap) AND get one certification (AWS Cloud Practitioner or Azure Fundamentals). You get the practical proof AND the credential.

### Filipino Tech Recruiters Who Look at Portfolios

- **Robert Walters Philippines** — Tech recruitment
- **Michael Page Philippines** — IT and engineering roles
- **JobsDB Philippines** — Tech job board
- **LinkedIn Philippines** — The most active platform for tech hiring
- **RemotePH** — Remote work opportunities for Filipinos

### Local Homelab Community

- **Facebook Groups:**
  - "Homelab Philippines" — Active community
  - "Self-Hosted Philippines" — Growing community
  - "Pinoy Homelabbers" — Beginner-friendly
- **Discord:** Several Filipino tech Discord servers have homelab channels
- **Meetups:** IT conferences in Manila, Cebu, and Davao sometimes have homelab talks

---

## Stress Test

Now let's prove your portfolio works:

1. **Ask a non-tech friend to look at your README.** Can they understand what you built? If not, simplify the language.
2. **Share your LinkedIn post with a recruiter or hiring manager.** See if they respond. Most will.
3. **Delete your GitHub repo and recreate it from memory.** If you can rebuild your portfolio, you truly understand what you built.
4. **Give a 5-minute verbal summary of your homelab.** Practice this. You'll need it in interviews.

> **🔥 The Chaos Champion:** Set up a mock interview with a friend. Tell them you're interviewing for a DevOps role. When they ask "What projects have you worked on?" give your homelab pitch. Record it. Watch it back. You'll be surprised how much better you sound the third time.

---

## What's Next

You've built something real. Something that teaches you skills, saves you money, and proves your abilities to the world. Your homelab is more than hardware and software — it's a **learning engine** and a **career accelerator**.

But the journey doesn't end here. There are always more services to deploy, more automation to write, more security to harden. The homelab is a bottomless well of learning.

**Homework:**
1. Write your "Homelab Story" (Step 1)
2. Take screenshots of your services (Step 2)
3. Create an architecture diagram (Step 3)
4. Set up a GitHub repository (Step 4)
5. Post about your homelab on LinkedIn (Deep Dive)

---

## Go Deeper

- [Feynman Technique](https://www.feynman-technique.com/) — Learn by teaching
- [Excalidraw](https://excalidraw.com/) — Diagrams for everyone
- [Awesome Self-Hosted](https://awesome-selfhosted.net/) — More services to explore
- [Homelab Forum](https://homelab.community/) — Community of homelab enthusiasts
- [r/homelab on Reddit](https://www.reddit.com/r/homelab/) — Largest homelab community

---

> **📢 Remember:** Your homelab is real work. Don't minimize it. Don't apologize for it. Own it. The skills you're building here are the same skills that power the companies you want to work for. The only difference is the address.
