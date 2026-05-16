# Hardware Buying Guide

_Recommended hardware for building your homelab, from lean to turbo._

---

## 🟢 Lean Path: ₱0-₱5,000

### Option A: Old Laptop (₱0)
- **Source:** Your existing laptop, a friend's old laptop, or Facebook Marketplace
- **Specs:** Any laptop from 2013+ with at least 4GB RAM and an SSD
- **Pros:** Built-in UPS (battery), portable, low power
- **Cons:** Limited RAM, not designed for 24/7 operation
- **Best for:** Trying homelabbing without spending anything

### Option B: Refurbished Mini PC (₱3,000-₱5,000)
- **Examples:** Lenovo ThinkCentre Tiny, Dell OptiPlex Micro, HP ProDesk Mini
- **Specs:** Intel Core i5 6th gen+, 8GB RAM, 128GB SSD
- **Where to buy:** Computer City (Manila), Facebook Marketplace, Shopee
- **Pros:** Small, quiet, efficient, enough power for Docker
- **Cons:** Limited upgradeability

---

## 🔵 Sweet Spot: ₱5,000-₱15,000

### Option A: Refurbished Micro/Mini Tower (₱5,000-₱8,000)
- **Examples:** Dell OptiPlex 3050/5050, HP ProDesk 400 G4, Lenovo ThinkCentre M720
- **Specs:** Intel Core i5/i7 7th-8th gen, 16GB RAM, 256GB SSD
- **Where to buy:** Computer City, Facebook Marketplace, eBay
- **Pros:** Great performance per peso, upgradeable RAM and storage
- **Cons:** Uses more power than a mini PC, slightly louder

### Option B: Second-hand NUC (₱8,000-₱15,000)
- **Examples:** Intel NUC 7i5, NUC 8i5, NUC 10i3
- **Specs:** Intel Core i5, 16GB RAM (upgradeable), 256GB+ SSD
- **Where to buy:** Facebook Marketplace, Shopee, Lazada
- **Pros:** Extremely compact, low power, quiet
- **Cons:** More expensive per spec, limited upgrade options

---

## 🟣 Turbo Path: ₱15,000-₱50,000

### Option A: New Mini PC (₱15,000-₱25,000)
- **Examples:** Beelink SER5, Minisforum UM560X, ACER Revo
- **Specs:** AMD Ryzen 5/7, 16-32GB RAM, 500GB+ SSD
- **Where to buy:** Shopee, Lazada, Lazada Mall
- **Pros:** Brand new warranty, excellent performance, low power
- **Cons:** Higher upfront cost

### Option B: Used Desktop + UPS (₱20,000-₱30,000)
- **Examples:** Dell OptiPlex 7050 Tower, HP Z2 Mini Workstation
- **Specs:** Intel Core i7, 32GB RAM, 1TB SSD, 2TB HDD
- **Add:** APC 600VA UPS (₱2,000-₱3,000)
- **Pros:** Maximum power, lots of storage, great for learning
- **Cons:** Uses more electricity, takes up space

---

## What Matters Most

| Priority | What to Look For | Minimum | Recommended |
|---|---|---|---|
| **RAM** | Memory for containers | 4GB | 8GB+ |
| **Storage** | SSD for speed | 64GB SSD | 256GB SSD |
| **CPU** | Processing power | Dual-core | Quad-core i5+ |
| **Network** | Ethernet port | WiFi only | Ethernet + WiFi |
| **Power** | Efficiency | Any | <30W idle |

**The hierarchy:** RAM > SSD > CPU. Don't skimp on RAM. An SSD is non-negotiable (no more spinning hard drives for your OS). CPU matters less — Docker doesn't need a supercomputer.

---

## Where to Buy in the Philippines

### Online
| Store | Best For | Notes |
|---|---|---|
| **Shopee** | New mini PCs, SSDs, accessories | Look for "Shopee Mall" sellers |
| **Lazada** | New mini PCs, UPS | Watch for flash sales |
| **Facebook Marketplace** | Used enterprise hardware | Negotiate, test before buying |
| **eBay** | International deals | Factor in shipping and customs |

### Physical Stores
| Store | Location | Best For |
|---|---|---|
| **Computer City** | Manila (Plaza Miranda) | Testing before buying, SSDs |
| **Makati Computer Mall** | Makati | Used enterprise gear |
| **Robinsons Tech** | Various malls | New mini PCs, warranty |

---

## Power Consumption

| Device | Idle Power | Under Load | Monthly Cost (₱) |
|---|---|---|---|
| Old laptop (2013) | 20-30W | 40-60W | ₱200-₱350 |
| Mini PC (i5 8th gen) | 8-15W | 25-40W | ₱100-₱200 |
| Desktop (i7, no GPU) | 40-60W | 80-120W | ₱400-₱700 |
| Raspberry Pi 4 | 3-5W | 6-8W | ₱40-₱80 |

*Based on ₱7/kWh (average PH electricity rate). Actual costs vary by ISP and usage.*

---

## Storage Guide

| Use Case | Minimum | Recommended | Cost (₱) |
|---|---|---|---|
| OS only (no data) | 64GB SSD | 128GB SSD | ₱500-₱1,000 |
| OS + a few services | 128GB SSD | 256GB SSD | ₱1,000-₱2,000 |
| Full homelab stack | 256GB SSD | 500GB SSD + 1TB HDD | ₱2,500-₱5,000 |
| Media server | 500GB SSD | 1TB SSD + 2TB HDD | ₱5,000-₱8,000 |

**SSD vs HDD for homelab:**
- **SSD** — Fast, silent, reliable. Use for your OS and Docker.
- **HDD** — Cheap, high capacity, noisy. Use for bulk storage (media, backups).
- **Hybrid** — SSD for OS + Docker, HDD for data. Best value.

---

## The UPS Question

Is a UPS (Uninterruptible Power Supply) worth it?

**Yes, if:**
- Your area has frequent brownouts
- You have important data that can't afford corruption
- You want your server to shut down gracefully during long outages

**No, if:**
- You're on a budget (start without one)
- Your area has stable power
- You're using a laptop (built-in battery)

**Recommended:** APC Back-UPS 600VA (₱2,000-₱3,000). Gives you 15-30 minutes of battery — enough for a graceful shutdown or to ride out short brownouts.

---

> **💸 The Lean Path Reminder:** The best server is the one you already own. Don't buy anything new unless you absolutely need to. Start with what you have, learn, and upgrade only when you hit a real limitation.
