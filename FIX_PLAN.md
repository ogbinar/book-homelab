# Fix Plan — Bahay Bahayan v1.0

> Generated: 2026-05-16
> Based on: End-to-end review findings
> Priority order: Critical → High → Medium → Low

---

## Critical Issues (Block Release)

### FIX-01: Chapter 13 Missing — Nav Jumps 12→14

**Problem:** `mkdocs.yml` nav goes from Ch 12 directly to Ch 14. No `chapter-13.md` exists. Ch 12 "What's Next" says "In Chapter 14, we'll document everything..." which skips a chapter number. Ch 14 references "Chapter 13 in v2" (leaked dev note, see FIX-09).

**Root cause:** Ch 13 was planned but never written.

**Fix:**
1. **Write Ch 13: "Remote Access — Your Lab, Anywhere"** as the bridge between Ch 12 (Security) and Ch 14 (Portfolio). Topic: Cloudflare Tunnel + Tailscale for CGNAT-friendly remote access — critical for PH context where most ISPs use CGNAT.
2. **Update mkdocs.yml:** Add `"Ch 13: Remote Access": chapters/chapter-13.md` after Ch 12.
3. **Update Ch 12 "What's Next":** Change to reference Ch 13.
4. **Update Ch 13 "What's Next":** Reference Ch 14.

**Why Ch 13 = Remote Access:**
- Ch 6 mentions "see Chapter 6 for remote access" but only covers LAN
- Ch 7 reverse proxy doesn't work with CGNAT (common in PH)
- Ch 12 security chapter naturally leads into "how to expose safely"
- Ch 14 portfolio needs remote screenshots
- Fits the narrative arc: you've built it, secured it, now access it from anywhere

**Ch 13 outline:**
- The Story: Accessing your homelab from a coffee shop, during commute
- Quick Start: Tailscale (easiest, works through CGNAT)
- The Why: How Cloudflare Tunnel vs Tailscale vs port forwarding compare
- Deep Dive: Cloudflare Tunnel for public-facing services
- PH Context: CGNAT is the default, not the exception
- Career Boost: Remote infrastructure access, zero-trust networking
- Stress Test: Kill your WiFi, try SSH via Tailscale from mobile data

---

### FIX-02: Garbled Text — Ch 1 Line 103

**Problem:** `docs/chapters/chapter-01.md:103` contains Chinese characters:
```
Even the hard stuff has an easy入门 point.
```

**Fix:** Replace with:
```
Even the hard stuff has an easy starting point.
```

---

### FIX-03: Garbled Text — Ch 4 Line 251

**Problem:** `docs/chapters/chapter-04.md:251` contains garbled characters:
```
- **Pi-hole:** Fdnf屏 Dashboard (mobile browser) + Admin Panel
```

**Fix:** Replace with:
```
- **Pi-hole:** Web Dashboard (mobile browser) + Admin Panel
```

---

### FIX-04: Ch 7 Caddy Compose — `network_mode: host` + External Network Conflict

**Problem:** `docs/chapters/chapter-07.md` Caddy compose file (lines 90-112) has both `network_mode: host` AND an external network definition:
```yaml
    network_mode: host

networks:
  default:
    external:
      name: bridge
```

These are contradictory. `network_mode: host` ignores all network configuration. The external network block is dead config that confuses readers.

**Fix:** Remove the `networks` block from the Caddy compose file. Keep `network_mode: host` (which is correct for Caddy binding to ports 80/443). Add a note explaining why `network_mode: host` is used.

**Same issue in Ch 8:** `docs/chapters/chapter-08.md:206` — Caddy service in the master compose also has `network_mode: host` while other services use the `homelab` network. This is actually CORRECT behavior (Caddy needs host networking), but the compose file has a `networks` section at the bottom that Caddy won't use. This is fine but should be documented with a comment.

---

### FIX-05: Ch 11 cAdvisor Port 8080 Conflicts with Vaultwarden

**Problem:** Ch 4 deploys Vaultwarden on port 8080 (`-p 8080:80`). Ch 11 deploys cAdvisor on port 8080 (`-p 8080:8080`). Port conflict.

**Fix:** Change cAdvisor to port `9100` or `8081` in Ch 11. Use `9101` to avoid conflicting with node-exporter's `9100`. Update the Prometheus scrape config to match.

```yaml
# In Ch 11, change:
    ports:
      - "8080:8080"
# To:
    ports:
      - "9101:8080"

# In prometheus.yml scrape config:
  - job_name: 'cadvisor'
    static_configs:
      - targets: ['cadvisor:8080']  # Internal port stays 8080
```

Note: The Prometheus scrape target uses the internal container port (`cadvisor:8080`), which is correct. Only the host mapping (`9101:8080`) needs to change.

---

## High Priority Issues

### FIX-06: Ch 5 / Ch 9 Backup Content Overlap

**Problem:** Both Ch 5 ("Keeping It Alive") and Ch 9 ("Your Data, Safe") teach backup setup. Ch 5 includes:
- Full backup script with `homelab-backup.sh`
- External USB drive mounting
- Cron scheduling
- Backup testing
- 3-2-1 backup rule explanation

Ch 9 covers the same topics but with a more advanced script. A reader doing Ch 5 has already set up backups, making Ch 9 feel redundant.

**Fix: Clarify the progression.**

**Ch 5 should teach:** Auto-restart policies + basic backup awareness (the "oh no, what if my data is gone?" moment). Keep it SHORT — just enough to not lose data.

**Ch 9 should teach:** Complete backup strategy (3-2-1 rule, offsite, Restic, verification drills).

**Specific changes to Ch 5:**
- Remove: Full backup script, USB mount instructions, cron scheduling, backup testing drill
- Replace with: Simple `docker cp` backup of current service data, manual copy to USB
- Add note: "This is a basic backup. Chapter 9 covers the full 3-2-1 backup strategy with automation."
- Keep: Auto-restart policies, health monitoring (Uptime Kuma), Docker cleanup

**Specific changes to Ch 9:**
- Add note at top: "If you set up basic backups in Chapter 5, you'll upgrade to the full strategy here."
- Keep all current content (it's the authoritative backup chapter)

---

### FIX-07: Ch 4 "Pick ONE" vs Ch 8+ Assumes All Services Deployed

**Problem:** Ch 4 says "Pick ONE of these four services." But Ch 8 master compose includes ALL services (vaultwarden, nextcloud, pihole, jellyfin). Ch 7 Caddyfile routes ALL services. A reader who followed the "pick one" instruction has only 1 service but Ch 8 expects 4-5.

**Fix: Update Ch 4 to encourage deploying at least 2-3 services.**

**Change Ch 4:**
- Change "Pick ONE" to "Pick at least ONE — we recommend starting with 2-3."
- Add: "You'll need multiple services for Chapter 8 (organizing everything into one system). Start with Vaultwarden (passwords) and one other. You can always add more later."

**Change Ch 8:**
- Add comment in compose file: "Remove any services you haven't deployed yet. This is a template — customize it to your setup."
- Add section: "Customizing the Stack" with instructions for removing services you don't have.

**Change Ch 7 Caddyfile:**
- Add comment: "Remove routes for services you haven't deployed. This Caddyfile is a template."

---

### FIX-08: Empty Appendix Stubs (5 of 7)

**Problem:** Five appendix files are empty stubs (title + one line description only):
- `glossary.md` — 3 lines
- `hardware.md` — 3 lines
- `errors.md` — 3 lines
- `services.md` — 3 lines
- `commands.md` — 3 lines

`author.md` (93 lines) and `community.md` (57 lines) are complete.

**Fix: Write content for all 5 stubs.**

#### Appendix B: Glossary
Extract all "Jargon Alert" terms from Ch 1-14 into an A-Z glossary. Terms to include (from reading):
- Idempotent, IP address, CIDR (/24), Default Gateway, NAT, Private IP, Public IP, Port, Reverse Proxy, ACME, Infrastructure as Code, RPO, RTO, Observability, Cron, CGNAT, DHCP, DNS, SSL/TLS, Defense in Depth, Portfolio, Container, Volume, Image

#### Appendix C: Hardware Buying Guide
- Budget tiers: ₱0 (old laptop), ₱5K-₱15K (refurb mini PC), ₱15K-₱50K (new NUC)
- Shopee/Lazada/Computer City sourcing tips
- What specs matter for homelab (RAM > CPU > Storage)
- Power consumption comparison table
- PSU/UPS recommendations for PH context

#### Appendix D: Common Errors
- Docker errors: "port already in use", "no space left on device", "container exited"
- Network errors: "connection refused", DNS resolution failures, CGNAT issues
- Permission errors: "permission denied", "access denied"
- Each error: symptom → cause → fix → prevention

#### Appendix E: Service Directory
- Table of all services mentioned in the book with: name, purpose, Docker image, default port, resource requirements, official docs link
- Include: Uptime Kuma, Vaultwarden, Nextcloud, Pi-hole, Jellyfin, Caddy, Prometheus, Grafana, node-exporter, cAdvisor, Watchtower, Restic
- Bonus section: "Services to explore next" (Gitea, Uptime Kuma alternatives, etc.)

#### Appendix F: Commands Reference
- Docker commands: `docker ps`, `docker logs`, `docker compose up/down`, etc.
- Linux commands: `ssh`, `lsblk`, `df`, `free`, `ip addr`, `systemctl`
- Network commands: `nmap`, `ping`, `curl`, `netstat`
- Each command: syntax, example, when to use

---

## Medium Priority Issues

### FIX-09: Ch 14 Leaked Dev Note — "Chapter 13 in v2"

**Problem:** `docs/chapters/chapter-14.md:340` contains:
```
4. **Personal website** — Once you build one (Chapter 13 in v2), link to it.
```

This is a leaked development note referencing a future version.

**Fix:** Replace with:
```
4. **Personal website** — Once you build one, link your homelab portfolio to it.
```

---

### FIX-10: agents.md References Incus/Proxmox — Book Uses Docker Only

**Problem:** `agents.md` (the style manifesto) mentions Incus and Proxmox multiple times as examples, but the book exclusively uses Docker. This creates confusion for AI agents generating content.

**Specific locations in agents.md:**
- Line 28: "Every `incus launch` explains the networking model."
- Line 46: "We use Incus over Proxmox VE for containers because..."
- Line 166-168: "Incus snapshot command"
- Line 177-178: "`incus network create` with these flags..."

**Fix: Update agents.md to Docker-only examples.**

Replace Incus references with Docker equivalents:
- "Every `incus launch`" → "Every `docker run`"
- "We use Incus over Proxmox VE" → "We use Docker over Podman"
- "Incus snapshot command" → "Docker commit/snapshot command"
- "`incus network create`" → "`docker network create`"

---

### FIX-11: Ch 7 Caddyfile Has Wrong Service Ports

**Problem:** `docs/chapters/chapter-07.md:140-157` Caddyfile references:
```
vault.homelab.local {
    reverse_proxy vaultwarden:80
}
```
Vaultwarden's internal port IS 80 (correct). But:
```
nextcloud.homelab.local {
    reverse_proxy nextcloud:80
}
```
Nextcloud's internal port IS 80 (correct). The Caddyfile is correct for internal Docker ports. No fix needed — confirmed correct.

**VERDICT: Not an issue. Marking for awareness only.**

---

### FIX-12: Ch 8 Compose — Vaultwarden Still on Port 8080 (Conflict with FIX-05)

**Problem:** Ch 8 master compose has vaultwarden on `-p 8080:80`. After FIX-05 applies (cAdvisor moves to 9101), this is no longer a conflict. But for consistency, consider moving Vaultwarden to a different port too.

**VERDICT: No fix needed after FIX-05. Port 8080 is fine for Vaultwarden since cAdvisor moves to 9101.**

---

### FIX-13: Ch 5 Backup Script — Hardcoded Service Names

**Problem:** Ch 5 backup script has `SERVICES="uptime-kuma vaultwarden nextcloud pihole jellyfin"` hardcoded. Reader who only deployed one service (per Ch 4 "pick one") will get errors for non-existent containers.

**Fix:** Add comment explaining to customize the SERVICES list. Change script to gracefully skip missing containers (it already does with `grep -q`, but add a note).

---

## Low Priority / Polish

### FIX-14: Ch 4 — Stress Test Mentions Mobile Data Access

**Problem:** Ch 4 stress test says "Turn off WiFi on your phone and use mobile data — Can you still access your service on your home network? (No, and that's expected — see Chapter 6 for remote access)."

But Ch 6 is about networking fundamentals, not remote access. Remote access isn't covered until Ch 13 (FIX-01).

**Fix:** Change reference to "see Chapter 13 for remote access" (after Ch 13 is created).

---

### FIX-15: Ch 6 — "What's Next" References Ch 7 Correctly

**Problem:** None. Ch 6 "What's Next" correctly references Ch 7 (reverse proxy). Confirmed.

**VERDICT: No fix needed.**

---

### FIX-16: MkDocs Nav — Add Ch 13 Entry

**Part of FIX-01.** After Ch 13 is written, add to `mkdocs.yml`:
```yaml
      - "Ch 13: Remote Access": chapters/chapter-13.md
```
Between Ch 12 and Ch 14.

---

### FIX-17: Part II Intro — Update to Reflect 14 Chapters

**Problem:** `docs/part-ii.md` references "Ch 8–14" but with Ch 13 added, the range is correct. No fix needed.

**VERDICT: No fix needed after FIX-01.**

---

## Execution Order

Execute fixes in this order to avoid cascading changes:

| Order | Fix | Reason |
|-------|-----|--------|
| 1 | FIX-02, FIX-03 | Quick text fixes, no dependencies |
| 2 | FIX-09 | Quick text fix before Ch 14 references change |
| 3 | FIX-05 | Port conflict fix, needed before Ch 8 compose review |
| 4 | FIX-04 | Caddy compose fix, affects Ch 7 and Ch 8 |
| 5 | FIX-07 | "Pick one" vs "all services" — affects Ch 4, 7, 8 |
| 6 | FIX-06 | Backup overlap — restructure Ch 5, update Ch 9 |
| 7 | FIX-01 | Write Ch 13 — largest task, updates nav and transitions |
| 8 | FIX-16 | MkDocs nav update (follows FIX-01) |
| 9 | FIX-10 | agents.md cleanup — doesn't affect content |
| 10 | FIX-08 | Write 5 appendix stubs — independent work |
| 11 | FIX-13, FIX-14 | Minor polish fixes |

---

## Estimates

| Category | Fixes | Estimated Effort |
|----------|-------|-----------------|
| Critical | FIX-01 to FIX-05 | ~4 hours (Ch 13 is ~2 hours) |
| High | FIX-06 to FIX-08 | ~5 hours (appendices are ~2 hours) |
| Medium | FIX-09 to FIX-13 | ~1 hour |
| Low | FIX-14 to FIX-17 | ~30 minutes |
| **Total** | **17 fixes** | **~10-12 hours** |
