# Hardware Buying Guide

_Recommended hardware for building your homelab, from lean to turbo — with prices, vendors, and compatibility guides._

> **Quick Reference:** Hardware setup — **[Chapter 2](../chapters/chapter-02.md)**. Power considerations — **[Chapter 5](../chapters/chapter-05.md)**. Storage planning — **[Chapter 8](../chapters/chapter-08.md)**. Remote access — **[Chapter 7](../chapters/chapter-07.md)**.

> **⚠️ Disclaimer:** Prices are estimates based on Q1 2026 market rates in the Philippines. Hardware prices change frequently — always verify before purchasing.

This appendix is intentionally PH-practical: it assumes budget pressure, second-hand parts, hot rooms, weak upload speeds, and the kind of power reality most home setups deal with here.

---

## 🟢 Lean Path: ₱0-₱15,000

### Option A: Old Laptop (₱0)
- **Source:** Your existing laptop, a friend's old laptop, or Facebook Marketplace
- **Specs:** Any laptop from 2013+ with at least 4GB RAM and an SSD
- **Pros:** Built-in UPS (battery), portable, low power, no extra hardware needed
- **Cons:** Limited RAM (usually not upgradeable past 8-16GB), battery degrades over time
- **Best for:** Trying homelabbing without spending anything
- **Power:** ~15-30W idle → ~₱100-₱200/month (Meralco at ₱7/kWh)

### Option B: Raspberry Pi 5 (₱7,000-₱12,000)
- **Models:**
  - Pi 5, 4GB RAM: ~₱7,000
  - Pi 5, 8GB RAM: ~₱9,000
  - Kit (with case, fan, power supply): ~₱10,000-₱12,000
- **Where to buy:**
  - Adafruit / Seeed Studio (international, +shipping)
  - **Robinsons Tech** — Manila, Cebu, Davao branches
  - **Shopee** — Search "Raspberry Pi 5 official store"
  - **Lazada** — Look for authorized resellers
- **Pros:** Tiny, quiet, extremely low power, huge community
- **Cons:** ARM architecture (some Docker images need `--platform linux/arm64`), USB 3.0 needs external hub, SD card not ideal for heavy writes
- **Best for:** Learning, light services (Pi-hole, Vaultwarden, Uptime Kuma), MQTT/IoT
- **Power:** ~3-5W idle → ~₱40-₱80/month

### Option C: Refurbished Mini PC (₱5,000-₱15,000)
- **Examples:**
  - Lenovo ThinkCentre Tiny M720q (i5-8500T): ~₱5,000-₱7,000
  - Dell OptiPlex 3050 Micro (i5-8500T): ~₱6,000-₱8,000
  - HP ProDesk 400 G4 DM (i5-8500T): ~₱5,500-₱7,500
  - Fujitsu Celsius h370 (i5-8500T): ~₱6,000-₱8,000
- **Typical specs:** Intel Core i5 8th gen+, 8-16GB RAM, 128-256GB SSD
- **Where to buy:**
  - **Computer City** (Manila, Plaza Miranda) — Test before buying, negotiate
  - **Facebook Marketplace** — Search "ThinkCentre Tiny," "OptiPlex Micro"
  - **Shopee** — Look for "refurbished mini PC" sellers with ratings
  - **Makati Computer Mall** — Used enterprise gear
- **Pros:** x86_64 (full Docker compatibility), small form factor, efficient, upgradeable RAM and SSD
- **Cons:** Limited to ~2 RAM slots, limited storage expansion, no GPU
- **Best for:** **THE sweet spot for PH beginners** — runs everything the book covers
- **Power:** ~8-15W idle → ~₱100-₱200/month

---

## 🔵 Sweet Spot: ₱15,000-₱40,000

### Option A: Refurbished Micro/Mini Tower (₱15,000-₱25,000)
- **Examples:**
  - Dell OptiPlex 7050 Tower (i5-8500/9500): ~₱12,000-₱18,000
  - HP ProTower 290 G4 (i5-8500): ~₱14,000-₱20,000
  - Lenovo ThinkCentre M720 Gen 1 Tower: ~₱15,000-₱22,000
- **Typical specs:** Intel Core i5/i7 8th-9th gen, 16-32GB RAM, 256GB-500GB SSD
- **Where to buy:** Computer City, Facebook Marketplace, eBay (factor shipping)
- **Pros:** Great performance per peso, upgradeable RAM (4+ slots), more storage bays, better cooling
- **Cons:** Uses more power than mini PC, takes up desk space, slightly louder fans
- **Best for:** Multi-service homelab with Nextcloud, media transcoding, databases
- **Power:** ~25-40W idle → ~₱300-₱500/month

### Option B: Second-hand NUC (₱15,000-₱25,000)
- **Examples:**
  - Intel NUC 8i5BEH (i5-8259U): ~₱15,000-₱20,000
  - Intel NUC 10i3FNH (i3-1005G1): ~₱12,000-₱18,000
  - Intel NUC 11i5TNH (i5-1135G7): ~₱20,000-₱30,000
- **Typical specs:** Intel Core i5, 16-32GB RAM (soldered on some models), 256GB+ M.2 SSD
- **Where to buy:** Facebook Marketplace, Shopee, Lazada
- **Pros:** Extremely compact, low power, quiet, Intel Quick Sync (good for media transcoding)
- **Cons:** Limited RAM upgradeability (some models have soldered RAM), limited storage (1-2 M.2 slots), no GPU
- **Best for:** Compact setups, quiet environments (apartment/bedroom), media server with Plex/Jellyfin
- **Power:** ~10-20W idle → ~₱150-₱250/month

### Option C: N100 Mini PC (₱8,000-₱15,000)
- **Examples:**
  - Beelink S12 Pro (N100, 16GB, 500GB): ~₱12,000-₱15,000 (new)
  - Trigkey/Noevio N100 mini PC: ~₱8,000-₱12,000 (Shopee/Lazada)
  - Used N100 units on FB Marketplace: ~₱7,000-₱10,000
- **Specs:** Intel N100 (4 cores, 4 threads), 8-16GB RAM, 256GB-500GB SSD
- **Where to buy:** Shopee, Lazada (often on flash sale), Facebook Marketplace
- **Pros:** Brand new with warranty, excellent power efficiency (6W TDP), performs like older i5s, quiet
- **Cons:** ARM alternatives exist but x86 is safer for Docker, limited to 2 displays, no GPU
- **Best for:** New-with-warranty buyers who want efficiency + performance
- **Power:** ~5-12W idle → ~₱70-₱150/month

---

## 🟣 Advanced Path: ₱40,000-₱100,000

### Option A: TrueNAS Build (₱40,000-₱70,000)
- **Build:**
  - CPU: Used Xeon E3-1230 v3 or i7-4770 (₱1,500-₱3,000)
  - Motherboard: Used Supermicro C7Z170-OCE or similar (₱3,000-₱5,000)
  - RAM: 32GB DDR4 ECC (₱3,000-₱5,000)
  - Boot: 256GB SSD (₱1,000-₱1,500)
  - Storage: 4x 4TB HDD (Seagate IronWolf / WD Red) = ₱20,000-₱28,000
  - Case: Fractal Design Node 804 or similar NAS case (₱5,000-₱8,000)
  - PSU: 400W 80+ Bronze (₱2,000-₱3,000)
  - NIC: Intel i350-T4 (4-port 1GbE) (₱2,000-₱3,000)
  - **Total: ~₱37,000-₱54,000**
- **Where to buy:** Computer City, FB Marketplace (used enterprise gear), Shopee/Lazada for new parts
- **Pros:** Massive storage, ECC RAM for data integrity, expandable, reliable
- **Cons:** More power (80-150W idle), louder (more fans/drives), complex setup
- **Best for:** NAS-first approach, media libraries, backup target, data hoarders
- **Power:** ~60-120W idle (depends on drives) → ~₱700-₱1,400/month

### Option B: 10GbE Lab Build (₱60,000-₱100,000)
- **Build:**
  - CPU: Used EPYC 7302P system or i7-8700K (₱15,000-₱25,000)
  - RAM: 64GB DDR4 (₱5,000-₱8,000)
  - Boot: 512GB NVMe SSD (₱2,500-₱4,000)
  - Storage: 2x 2TB NVMe (cache/VMs) + 4x 4TB HDD (data) (₱20,000-₱30,000)
  - 10GbE NIC: Used Mellanox ConnectX-3 (₱2,000-₱3,000 on FB)
  - Switch: Used Cisco SG300-52 (₱8,000-₱12,000)
  - Case: Fractal Design Define 7 or similar (₱7,000-₱10,000)
  - PSU: 650W 80+ Gold (₱4,000-₱6,000)
  - UPS: APC 1500VA (₱6,000-₱8,000)
  - **Total: ~₱62,500-₱96,000**
- **Where to buy:** Computer City, FB Marketplace (used networking gear), Shopee/Lazada
- **Pros:** Blazing fast storage, enterprise networking, future-proof
- **Cons:** Expensive, power-hungry (150-250W idle), loud, overkill for most
- **Best for:** Serious homelabbers, VM-heavy workloads, learning enterprise networking
- **Power:** ~100-200W idle → ~₱1,200-₱2,400/month

---

## 📊 Hardware Comparison Matrix

### Performance vs Power

| Device | CPU | RAM | Idle Power | Under Load | Monthly (₱) | Best For |
|---|---|---|---|---|---|---|
| **Raspberry Pi 5 (8GB)** | ARM v8 | 8GB | 3-5W | 6-8W | ₱40-₱80 | Learning, light services |
| **N100 Mini PC** | Intel N100 | 8-16GB | 5-12W | 20-35W | ₱70-₱150 | Best value new |
| **ThinkCentre Tiny M720q** | i5-8500T | 8-16GB | 8-15W | 40-65W | ₱100-₱200 | **Sweet spot** |
| **Intel NUC 8i5BEH** | i5-8259U | 16GB | 10-20W | 50-70W | ₱150-₱250 | Compact + Quick Sync |
| **OptiPlex 7050 Tower** | i5-8500 | 16-32GB | 25-40W | 80-120W | ₱300-₱500 | Multi-service homelab |
| **TrueNAS Build (4 drives)** | Xeon E3 | 32GB | 60-120W | 100-180W | ₱700-₱1,400 | NAS-first, media |
| **10GbE Lab** | EPYC/i7 | 64GB | 100-200W | 200-350W | ₱1,200-₱2,400 | Enterprise learning |

*Meralco rate: ~₱7/kWh (varies by usage tier and surcharges)*

### Noise Levels for Apartment Living

| Device | Noise Level | Apartment-Friendly? |
|---|---|---|
| Raspberry Pi 5 | Silent (passive cooling on some models) | ✅ Yes |
| N100 Mini PC | Whisper quiet (fan only under load) | ✅ Yes |
| ThinkCentre Tiny | Quiet (small fan, spins down) | ✅ Yes |
| Intel NUC | Quiet (small form factor cooling) | ✅ Yes |
| OptiPlex Tower | Moderate (standard desktop fans) | ⚠️ OK, not bedroom |
| TrueNAS Build | Loud (multiple drives + fans) | ❌ No, separate room |
| 10GbE Lab | Loud (high airflow, multiple drives) | ❌ No, separate room |

### Heat Management in Tropical Climate

The Philippines' tropical climate means your homelab runs warmer than in temperate climates. Here's what matters:

| Factor | Consideration | Mitigation |
|---|---|---|
| **Ambient temp** | 27-32°C average year-round | Ensure at least 10cm clearance around your device |
| **Humidity** | 70-85% average | Use in AC room if possible; avoid floor placement |
| **Dust** | More AC = more dust circulation | Clean filters/fans monthly |
| **Power stability** | Brownouts common | UPS recommended for all setups above ₱5,000 |

**Cooling recommendations:**
- Mini PCs and NUCs: Fine in most indoor environments. No extra cooling needed.
- Tower builds: Ensure front intake has dust filter. Clean every 2-3 months.
- NAS builds: Consider ambient temperature monitoring (`smartctl -A /dev/sda | grep Temperature`). If drive temp exceeds 45°C, add case fans.
- Raspberry Pi: The active cooler model handles heat well. Passive cooling may throttle under sustained load.

---

## 🛒 PH Vendor Directory

### Physical Stores

| Store | Location | Best For | Notes |
|---|---|---|---|
| **Computer City** | Manila (Plaza Miranda, 3F building) | Testing before buying, SSDs, RAM, used enterprise gear | Negotiate! Prices vary by floor. 3F has the best used server deals. |
| **Makati Computer Mall** | Makati (4F) | Used enterprise gear, networking equipment | Ask for "wholesale" prices if buying multiple items |
| **Robinsons Tech** | Various malls (SM branches) | New mini PCs, warranty items, accessories | Fixed prices but frequent sales. Check for open-box deals. |
| **Silicon Analytics** | Makati, BGC, Cebu | New components, developer boards (Raspberry Pi, etc.) | Authorized reseller, genuine products with warranty |
| **V-Store** | Various locations | New laptops, mini PCs, peripherals | Often has promos. Good for warranty claims. |
| **Octagon Micro** | Manila, QC, Cebu | New components, GPUs, motherboards | Price-match guarantee. Check their website for stock. |

### Online Sellers

| Store | Best For | Trust Indicators |
|---|---|---|
| **Shopee** | New mini PCs, SSDs, accessories | Look for "Shopee Mall" badge, 95%+ rating, 1000+ followers |
| **Lazada** | New mini PCs, UPS, peripherals | Look for "LazMall" badge, flash sales |
| **Facebook Marketplace** | Used enterprise hardware | Meet in public, test before paying, bring a friend |
| **Carousell PH** | Used components, networking gear | User ratings, verified accounts |

### Second-Hand Buying Tips

**Red flags on Facebook Marketplace:**
- ❌ Seller won't meet in person
- ❌ Prices way below market (scam)
- ❌ No photos of the actual unit
- ❌ Seller rushes you to decide
- ❌ Payment upfront before seeing the item

**Inspection checklist when buying used:**
1. **Power on the unit** — Does it boot to BIOS/OS?
2. **Check RAM** — `sudo dmidecode -t memory` (how much, what speed, how many slots used)
3. **Check storage** — `lsblk` (what drives are installed)
4. **Stress test** — Run `stress --cpu 4 --timeout 60` for 60 seconds. Does it throttle or crash?
5. **Check thermal** — `sensors` or `sudo smartctl -a /dev/sda | grep Temperature`
6. **Check ports** — USB, Ethernet, HDMI — do they all work?
7. **Ask for receipt** — Proves purchase date, helps with warranty claims

---

## 📋 Complete Shopping Lists (BOM)

### Build 1: "Starter Homelab" (₱12,000-₱15,000)

Perfect for beginners who want a dedicated machine.

| Item | Specs | Est. Price | Where |
|---|---|---|---|
| Mini PC | Lenovo ThinkCentre M720q (i5-8500T, 8GB, 256GB SSD) | ₱6,500 | FB Marketplace / Computer City |
| RAM upgrade | 16GB DDR4 SO-DIMM (2x8GB) | ₱1,500 | Silicon Analytics / Shopee |
| SSD upgrade | 512GB NVMe SSD (Kingston NV2 or similar) | ₱2,000 | Shopee / Lazada |
| Ethernet cable | Cat6, 1m | ₱100 | Any computer store |
| USB drive | 32GB (for Ubuntu installer) | ₱150 | Any computer store |
| **Total** | | **~₱10,250** | |

Add a UPS later (₱2,000-₱3,000) when you're ready.

### Build 2: "Proxmox Home Server" (₱25,000-₱35,000)

For those ready to run VMs alongside containers.

| Item | Specs | Est. Price | Where |
|---|---|---|---|
| Tower PC | Dell OptiPlex 7050 (i5-8500, 16GB, 256GB SSD) | ₱15,000 | Computer City / FB |
| RAM upgrade | 32GB DDR4 (2x16GB) | ₱3,000 | Shopee / Octagon |
| Boot SSD | 512GB SATA SSD | ₱2,000 | Shopee |
| Data HDD | 2TB HDD (Seagate Barracuda) | ₱3,000 | Computer City |
| UPS | APC Back-UPS 600VA | ₱2,500 | Lazada / Robinsons |
| Ethernet cable | Cat6, 2m | ₱100 | Any store |
| **Total** | | **~₱25,600** | |

### Build 3: "NAS-First" (₱45,000-₱60,000)

Storage-first approach. Run services on top of your NAS.

| Item | Specs | Est. Price | Where |
|---|---|---|---|
| CPU | Used Xeon E3-1230 v3 | ₱2,000 | FB Marketplace |
| Motherboard | Supermicro C7Z170-OCE | ₱4,000 | FB Marketplace |
| RAM | 32GB DDR4 ECC | ₱4,000 | Shopee (non-ECC works too) |
| Boot SSD | 256GB SATA SSD | ₱1,000 | Shopee |
| Case | Fractal Design Node 804 | ₱6,000 | Shopee (import) |
| HDD x4 | 4TB IronWolf (WD Red works too) | ₱24,000 | Lazada / Octagon |
| PSU | 400W 80+ Bronze | ₱2,500 | V-Store / Octagon |
| NIC | Intel i350-T4 (4-port 1GbE) | ₱2,500 | FB Marketplace |
| **Total** | | **~₱46,000** | |

---

## 🔋 UPS Recommendations

A UPS (Uninterruptible Power Supply) is one of the best investments for a homelab in the Philippines. Brownouts are common, and an unprotected server can corrupt its filesystem.

| UPS Model | Capacity | Runtime (Mini PC) | Price (₱) | Best For |
|---|---|---|---|---|
| **APC Back-UPS 350VA** | 350VA / 200W | 10-15 min | ₱1,500-₱2,000 | Raspberry Pi, N100 mini PC |
| **APC Back-UPS 600VA** | 600VA / 360W | 15-30 min | ₱2,000-₱3,000 | Mini PC + router + switch |
| **APC Back-UPS 900VA** | 900VA / 540W | 20-45 min | ₱3,500-₱4,500 | Tower PC + NAS |
| **APC Smart-UPS 1500VA** | 1500VA / 1000W | 30-60+ min | ₱8,000-₱12,000 | Serious homelab, TrueNAS |

**Where to buy:** APC is widely available on Lazada, Shopee, and Robinsons Tech. Beware of counterfeits — buy from authorized dealers only.

**Pro tip:** Configure your server to shut down gracefully when the UPS battery hits 10%. Most UPS models support USB connection to the server. On Ubuntu, install `nut` (Network UPS Tools) to automate this.

---

## 💸 Meralco Cost Calculator

Here's how to estimate your monthly electricity cost:

```
Power (W) × 24 hours × 30 days ÷ 1000 = kWh per month

Cost = kWh × Meralco rate (approx. ₱7/kWh)
```

**Examples:**

| Setup | Idle Power | Monthly kWh | Monthly Cost (₱) | Annual Cost (₱) |
|---|---|---|---|---|
| Raspberry Pi 5 | 5W | 36 | ₱250 | ₱3,000 |
| N100 Mini PC | 10W | 72 | ₱500 | ₱6,000 |
| ThinkCentre Tiny | 12W | 86 | ₱600 | ₱7,200 |
| OptiPlex Tower | 30W | 216 | ₱1,500 | ₱18,000 |
| TrueNAS (4 drives) | 80W | 576 | ₱4,000 | ₱48,000 |

> **💡 Quick Win:** A N100 mini PC at ₱12,000 costs only ₱600/month in electricity. Over 3 years, that's ₱21,600 in power — still cheaper than most cloud hosting subscriptions.

---

## 🧠 Compatibility Quick Reference

### Docker Image Architecture

| Architecture | Compatible With | Not Compatible With |
|---|---|---|
| **x86_64 (amd64)** | All mini PCs, towers, NUCs | Raspberry Pi |
| **ARM64 (aarch64)** | Raspberry Pi 4/5, Orange Pi | Some older Docker images |

> **Note:** Most Docker images support both architectures and auto-detect. But some niche images are x86_64 only. If you buy a Raspberry Pi, check Docker Hub for ARM support before committing to a service.

### RAM Requirements by Workload

| Workload | Minimum RAM | Recommended RAM |
|---|---|---|
| Lite (Pi-hole, Uptime Kuma) | 2GB | 4GB |
| Standard (6-8 services) | 8GB | 16GB |
| Heavy (Nextcloud, media, VMs) | 16GB | 32GB |
| Enterprise (Proxmox, 10GbE) | 32GB | 64GB+ |

### Storage Tiers

| Tier | Use Case | Type | Capacity |
|---|---|---|---|
| OS + Docker | Operating system, containers | SATA SSD | 256GB+ |
| Data | Nextcloud, databases, media | HDD | 2TB+ |
| Cache (optional) | VMs, databases, frequent reads | NVMe SSD | 512GB+ |

**The hierarchy:** SSD for OS > SSD for Docker > HDD for data. Never use a spinning HDD for your operating system.

---

## When to Upgrade

You don't need to plan your upgrade. Start with what you have. Here's when it's time to upgrade:

1. **RAM is full** — `free -h` shows less than 10% free consistently → Upgrade RAM first (cheapest upgrade)
2. **Storage is full** — `df -h` shows less than 20% free → Add a drive, don't replace yet
3. **CPU is maxed out** — `htop` shows sustained 100% usage → Upgrade to next tier
4. **You need more services** — Your current setup handles what you need → You're not ready to upgrade yet

> **💸 Lean Path Reminder:** The best server is the one you already own. Don't buy anything new unless you absolutely need to. Start with what you have, learn, and upgrade only when you hit a real limitation. A homelab on a ₱5,000 mini PC is infinitely more valuable than a homelab plan for a ₱100,000 setup that never gets built.
