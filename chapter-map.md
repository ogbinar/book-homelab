# Bahay Bahayan Chapter Map

This is the revision map for incorporating the newer organizing ideas without losing the book's current identity.

## How to Read This Map

- `Prep -> Postdoc` is the maturity ladder.
- Household metaphors stay light and practical.
- Stakeholder thinking stays brief: `self`, `family`, `public`, `service management`.
- Game hosting, Dokploy-style operations, and local hosted LLMs are real additions, but they should not override the core book.

---

## Part I: Bahay Bahayan

### Chapter 1: What Even Is a Homelab?

**Keep:**
- the core emotional hook
- the anti-gatekeeping tone
- the Filipino-first framing

**Add / adjust:**
- one short note that the book grows by maturity level, not by prestige
- a simple mention that homelabs map to real household needs
- a light stakeholder reminder: who is this for, who manages it, who should not touch it

**Do not add:**
- heavy technical architecture
- advanced service taxonomy
- long future-path explanations

### Chapter 2: Your First Server

**Keep:**
- budget paths from `₱0` upward
- reused hardware as a valid start
- the idea that a laptop counts

**Add / adjust:**
- stronger low-power framing
- a clearer link between hardware choice and household constraints
- optional mention that the best homelab often starts as a quiet box in a real room, not a fantasy rack

**Do not add:**
- game hosting
- Dokploy
- local LLMs

### Chapter 3: Hello, Container

**Keep:**
- Docker as the first deep technical unlock
- the shipping-container analogy
- the first real service deployment

**Add / adjust:**
- the idea that containers are the toolbox for everything later
- a small note that organized service management starts here
- one line that the reader is building a house of services, not random one-off commands

**Do not add:**
- managed platform tools
- advanced orchestration
- AI-specific content

### Chapter 4: Something Useful

**Keep:**
- service choice as the first meaningful win
- Vaultwarden, Nextcloud, Pi-hole, Jellyfin
- the subscription-replacement angle

**Add / adjust:**
- a very light `self / family / public / service management` lens for the service the reader chooses
- a subtle setup for the `household service` metaphor
- a small forward reference that some services grow into family-wide or home-wide utility

**Where future ideas begin:**
- `Jellyfin` becomes the bridge to game/media household use
- `Nextcloud` becomes the bridge to household storage and personal knowledge
- `Pi-hole` becomes the bridge to trust boundaries and service exposure

**Do not add:**
- full game server walkthroughs
- Dokploy
- local hosted LLMs as a full section

### Chapter 5: Keeping It Alive

**Keep:**
- auto-restart
- health monitoring
- cleanup
- basic reliability language

**Add / adjust:**
- stronger brownout and outage awareness
- a household continuity lens
- a short reminder that a service is only useful if it survives interruptions
- an early hint that power resilience is one of the most homelab-specific themes in the whole book

**Do not add:**
- full UPS theory yet
- battery chemistry deep dive
- advanced backup architecture beyond what the chapter already needs

### Chapter 6: Your First Network

**Keep:**
- IPs, DNS, ports, routers
- the plumbing analogy
- local access from another device

**Add / adjust:**
- stronger framing for the home as a networked space with different trust levels
- a light stakeholder lens if useful for explaining who sees what
- a gentle setup for reverse proxy and public vs private exposure later

**Do not add:**
- remote access details that belong in Chapter 7
- game hosting
- local AI

---

## Part II: Lumaki

### Chapter 7: The Reverse Proxy + Remote Access

**Keep:**
- reverse proxy as the concierge/front desk
- clean URLs
- SSL and internal routing

**Add / adjust:**
- make this the main home for CGNAT reality
- keep remote access as a signature Philippine-context problem
- lightly distinguish private-only, family-accessible, and public-facing services
- mention that some services should stay behind tunnels instead of being directly exposed

**Best place for:**
- Tailscale / Cloudflare Tunnel style thinking
- public vs private boundary language
- a tiny stakeholder lens for exposure and trust

**Do not add:**
- game hosting as a main topic
- Dokploy
- local LLMs

### Chapter 8: Multiple Services, One System

**Keep:**
- Docker Compose as the organizational shift
- shared networks and volumes
- environment variables and stack thinking

**Add / adjust:**
- this becomes the first natural place to talk about service management as a household operations problem
- add a short bridge to `Dokploy-style control` as a later evolution of this chapter
- make the chapter feel like the utility cabinet of the house

**Best place for:**
- service organization
- deployment discipline
- raw Compose vs managed control plane comparisons

**Do not add:**
- a full Dokploy tutorial yet
- game hosting
- local AI

### Chapter 9: Your Data, Safe

**Keep:**
- 3-2-1 backup thinking
- restore drills
- offsite redundancy

**Add / adjust:**
- stronger storage architecture framing
- more direct links between backup planning and household continuity
- a light reminder that backup is not the same thing as storage design

**Best place for:**
- practical recovery planning
- restore verification
- the first serious `Masters`-level reliability thinking

**Do not add:**
- deep ZFS architecture unless it is clearly beginner-safe
- enterprise-style storage abstraction

### Chapter 10: Automation: Work Less

**Keep:**
- Bash scripts
- cron
- Watchtower
- maintenance loops

**Add / adjust:**
- household maintenance framing: automate the boring, not the important
- note that automation is the first step toward service management discipline
- a small bridge to future orchestration and control-plane thinking

**Do not add:**
- platform engineering jargon
- complex workflow tooling that steals focus from the home use case

### Chapter 11: Monitoring: Eyes on Everything

**Keep:**
- Uptime Kuma
- Prometheus / Grafana as advanced monitoring
- alerts and dashboards

**Add / adjust:**
- make the `house alert system` metaphor visible
- keep family-safe and home-safe signals in mind
- remind the reader that monitoring is for household awareness, not dashboard vanity

**Do not add:**
- enterprise observability deep theory
- logging and tracing bloat unless it is tightly tied to home use

### Chapter 12: Security: Don't Get Hacked

**Keep:**
- SSH keys
- UFW
- fail2ban
- service hardening

**Add / adjust:**
- strengthen the idea that public exposure is a deliberate choice
- keep the `locks and gates` metaphor
- lightly reinforce the stakeholder lens: some services are for self only, some for family, some should never be public

**Best place for:**
- public/private boundary rules
- a tiny reminder that `service management` is part of security

**Do not add:**
- heavy zero-trust theory
- enterprise identity management unless it is clearly relevant

### Chapter 13: Your Homelab Portfolio

**Keep:**
- career translation
- documentation
- screenshots, diagrams, and explainability

**Add / adjust:**
- make the narrative sound like maintaining a real household system
- keep the language simple and professional
- use the school ladder to show growth from `Prep` to `Builder`

**Do not add:**
- Part III topics in depth
- overly technical architecture that distracts from the portfolio goal

---

## Part III: Malaki

This part stays future-facing, but the structure should be more explicit so readers know where the advanced branches go.

### Chapter 14: Choose Your Path

**Keep:**
- the specialization idea
- the honest “you don’t need everything” message

**Add / adjust:**
- add explicit pathways for:
  - `Games`
  - `AI`
  - `Operations`
- keep the existing DevOps, Security, Media, Privacy, and IoT paths
- make the chapter the formal handoff from core homelab to advanced specialization

### Chapter 15: Kubernetes for Real Humans

**Keep:**
- only if it stays honest and scoped

**Add / adjust:**
- keep it as optional advanced architecture
- make sure it stays clearly outside the core path

### Chapter 16: Local AI: Your Own Intelligence

**Keep:**
- local LLM basics
- Ollama / RAG style work

**Add / adjust:**
- make this the main home for local hosted LLMs
- frame it as a private household assistant, not cloud hype
- keep hardware guidance honest and budget-aware

### Chapter 17: Game Room: Hosting What the House Uses

**Keep:**
- this is the main home for game hosting

**Add / adjust:**
- latency, direct access, recovery, and household use
- the idea that not all services should be tunneled
- the idea that some home services are meant to be shared with family or friends, not the public

### Chapter 18: The Operations Desk

**Keep:**
- this is the main home for Dokploy-style service control

**Add / adjust:**
- compare raw Compose, managed deployments, and lifecycle operations
- keep it grounded in homelab service management, not platform engineering
- show how a grown-up homelab reduces manual effort without losing local control

### Chapter 19: Home Automation

**Keep:**
- Home Assistant
- sensors, scenes, dashboards

**Add / adjust:**
- keep it local-first
- tie it to the household service metaphor

### Chapter 20: High Availability

**Keep:**
- only as an advanced optional topic

**Add / adjust:**
- make sure it stays honest about cost and complexity

### Chapter 21: Infrastructure as Code

**Keep:**
- reproducibility and documentation discipline

**Add / adjust:**
- keep it tied to the home lab, not abstract enterprise automation

### Chapter 22: Community & Contribution

**Keep:**
- community growth and sharing

**Add / adjust:**
- keep it grounded in Filipino community support and reader sharing

### Chapter 23: What's Next?

**Keep:**
- future vision

**Add / adjust:**
- point readers toward the advanced branches without making them feel incomplete if they stop earlier

---

## Editorial rules for the revision

- Do not rewrite the current voice into something new.
- Do not remove Taglish where it already works naturally.
- Do not make the book more formal than it already is.
- Do not make the book more casual than it already is.
- Do not add a new framework chapter for the stakeholder lens.
- Do not let game hosting, Dokploy, or local LLMs dominate the main book.
- Do let them exist as natural next steps in the architecture.

---

## Done when

- The current manuscript still feels like `Bahay Bahayan`.
- The new maturity ladder is visible across the book.
- Household metaphors make the advanced ideas easier to remember.
- Stakeholder thinking appears lightly and only where useful.
- Game hosting, Dokploy-style operations, and local hosted LLMs have a clear place in the structure.
- The manuscript remains simple, practical, Filipino-first, and complete.
