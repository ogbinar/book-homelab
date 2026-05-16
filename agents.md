# 📜 Agents.md: The Homelab Book Manifesto & Style Governance

This document defines the identity, voice, and structural integrity of the
**Book Homelab** guide—a living manifesto for transforming tech consumers into
infrastructure architects. All contributors and AI agents assisting in the
creation of this work must adhere to these "Prime Directives."

---

## 1. The Core Identity: "The Master Craftsman"

### The Persona

A seasoned homelab architect who isn't afraid to get their hands dirty.
We value the "clack" of a mechanical keyboard, the hum of a well-cooled rack,
and the satisfaction of a clean `systemctl status` at 2:00 AM.

### The Mission

To transform **consumers of tech** into **architects of infrastructure**
—specifically for Filipino data professionals and career shifters who need
practical, affordable, and privacy-first skills.

### The Promise

We don't just provide "copy-paste" commands; we provide the **intuition**
behind the configuration. Every `docker run` comes with a "why."
Every `docker network create` explains the networking model.

---

## 2. The Voice & Tone (Personality)

### Empathetic but Candid

- We acknowledge that networking is hard and DNS is always the problem
- We validate the frustration of a kernel panic but push the reader to find
  the "why"
- Example: *"Yeah, your first NAT bridge will break. Mine did too.
  Here's exactly what went wrong..."*

### Opinionated (No Neutrality)

- If a specific tool is bloated or "cloud-washed," say so
- Provide a **"Why We Use This"** section for every major tech choice
- Example: *"We use Docker Compose over Kubernetes for homelabs because..."*

### Witty & Grounded

- Use analogies from the physical world (plumbing, gardening, carpentry)
- Avoid "corporate-speak" (e.g., use "Let's get this running" instead of
  "Let's leverage this solution")
- Example: *"Think of your reverse proxy like a building concierge—
  directing visitors to the right apartment without giving them the master
  key."*

### Anti-Gatekeeping

- We speak to the "Career Shifter." If a term is jargon, define it once
  with a **"Jargon Alert"** and then use it professionally
- Example: *"Jargon Alert: 'Idempotent' just means 'running this command
  10 times has the same result as running it once.' Now you know."*

---

## 3. Structural Rules (The "Lab" Framework)

Every chapter or module **must** follow the **Template Structure** below.
This structure is optimized for Filipino beginners — it starts with a story
and builds to technical depth.

### Standard Chapter Structure

```markdown
# Chapter N: [Title]

## What You'll Build
[One-paragraph description of the outcome]

## How Long It Takes
[Realistic time estimate]

## What You Need
- [Prerequisites list]

---

## The Story
[Narrative hook — relatable scenario, Filipino context]

## Why This Matters
[Career and technical value]

## 🟢 Quick Start
[Hands-on steps — the "how"]

## 🔵 The Why
[Conceptual understanding — the "why"]

## 🟣 Deep Dive
[Advanced topics for those who want more]

## 💼 Career Boost
[How this translates to career value — CV entries, interview talking points]

## 🇵🇭 PH Context
[Filipino-specific considerations: costs, ISPs, hardware, culture]

## Stress Test
[How to intentionally break the setup to prove it works]

## What's Next
[Transition to next chapter]

## Go Deeper
[Further reading links]
```

### Section Requirements

| Section | Required? | Notes |
| --- | --- | --- |
| What You'll Build | ✅ Yes | Clear outcome statement |
| How Long It Takes | ✅ Yes | Realistic estimate |
| What You Need | ✅ Yes | Prerequisites list |
| The Story | ✅ Yes | Narrative hook, Filipino context |
| Why This Matters | ✅ Yes | Career + technical value |
| Quick Start | ✅ Yes | Hands-on steps with verification |
| The Why | ✅ Yes | Conceptual understanding |
| Deep Dive | ✅ Yes | Advanced/optional topics |
| Career Boost | ✅ Yes | CV entries, interview talking points |
| PH Context | ✅ Yes | Philippine-specific considerations |
| Stress Test | ✅ Yes | "Break it to prove it works" |
| What's Next | ✅ Yes | Transition to next chapter |
| Go Deeper | ✅ Yes | Further reading links |

### Code Block Standards

- Always include filename/context comment on first line
- Use language-specific syntax highlighting
- No truncated code blocks (use `...` only for emphasis, not actual omission)

---

## 4. Technical Constraints & Standards

### Privacy First

- All guides must prioritize local-first solutions
- If a cloud service is used, it must be behind a **"Local-to-Cloud Bridge"**
  (e.g., local processing, cloud logging)
- Example: *"We'll set up MinIO locally first. Only then do we add S3 sync
  as an optional backup."*

### Hardware Agnostic (Mostly)

- While we love high-spec gear (Strix Halo, dual Xeon rigs), always provide
  a **"Budget/Refurbished"** alternative path
- Example: *"Turbo: 64GB RAM? Run 20 containers. Lean Path: 8GB? Here's how
  to prioritize."*

### Documentation as Code

- All infrastructure steps should be accompanied by a **"Reproducibility
  Check"**:
  - Docker Compose file
  - Docker snapshot/commit command
  - Ansible playbook snippet
- Version pin everything

### No "Magic" Buttons

- Avoid GUI-only tutorials
- If a GUI is used, explain the CLI/API equivalent happening in the
  background
- Example: *"The 'Create Network' button you just clicked ran `docker network
create` with these flags..."*

---

## 5. Visual & Markdown Identity

### Callout Boxes (Standardized Icons)

Use these **exact** formats for consistency.
There are 8 callout types in the book:

```markdown
> **🟢 Quick Start:** [Section header — used for the main hands-on steps]
> NOT a callout box — it's a section header.

> **🔵 The Why:** [Section header — used for conceptual understanding]
> NOT a callout box — it's a section header.

> **🟣 Deep Dive:** [Section header — used for advanced topics]
> NOT a callout box — it's a section header.

> **💼 Career Boost:** [Section header — used for career value]
> NOT a callout box — it's a section header.

> **🇵🇭 PH Context:** [Section header — used for Filipino context]
> NOT a callout box — it's a section header.

> **🚀 Turbo:** Optimization tips for high-performance hardware.
> Example: With 128GB RAM, you can run the entire stack without swap.

> **⚠️ The Pitfall:** Common errors we've personally made.
> Example: I spent 3 hours debugging this before realizing the firewall was
> dropping UDP.

> **💸 The Lean Path:** How to do this for ₱0 or on old hardware.
> Example: That ₱15,000 NUC? You can do this on a ₱3,000 Core i3 from 2015.

> **🧠 The Deep Dive:** Theoretical context.
> Example: How the Linux kernel handles network namespaces at the packet level.

> **📢 Jargon Alert:** "Term" — Plain-language explanation.
> Example: "Idempotent" just means running this command 10 times has the
> same result as running it once.

> **⚠️ Watch Out:** Warning about something that could go wrong.
> Example: Before disabling password login, make sure your SSH key works first.

> **💡 Quick Win:** A small tip that provides immediate value.
> Example: Start using Docker Compose from the beginning.

> **🔥 The Chaos Champion:** An advanced stress test challenge.
> Example: Unplug your router, wait 30 seconds, plug it back in.
> See what breaks.
```

### Callout Usage Rules

1. **Section headers** (Quick Start, The Why, Deep Dive, Career Boost,
   PH Context) appear once per chapter as section titles
2. **Inline callouts** (Turbo, Pitfall, Lean Path, Jargon Alert, Watch Out,
   Quick Win, Chaos Champion) appear throughout the chapter body
3. **Every chapter must include at least one** of each inline callout type
   where contextually appropriate
4. **Jargon Alert** must appear whenever a technical term is first introduced
5. **Career Boost** section must always include both "What employers care
   about" bullet points AND an "Interview talking point" blockquote

### Jargon Alerts

```markdown
> **📢 Jargon Alert:** "BGP" (Border Gateway Protocol) is how the
> internet's routers talk to each other.
> Think of it as the postal system for IP packets.
```

### Code Block Formatting

- Always include filename/context comment on first line
- Use language-specific syntax highlighting
- No truncated code blocks (use `...` only for emphasis, not actual omission)

---

## 6. The "Career Accelerator" Rule

Every technical tutorial must answer:
**"How do I put this on my CV?"**

### Career Boost Section Format

At the end of every chapter (except Chapter 1), include a **Career Boost**
section with:

```markdown
---

## 💼 Career Boost

**What you learned that employers care about:**
- [Skill 1]
- [Skill 2]
- [Skill 3]

**Interview talking point:**
> *"I designed and maintained [infrastructure] using [tools], achieving
> [result]. The infrastructure includes [components]."*

(Remove this template text and replace with actual content.)
```

### CV Translation Table

When appropriate, include a translation table showing how homelab terms map
to professional language:

| Homelab Term | Professional Term |
| --- | --- |
| "I run a homelab" | "I manage self-hosted infrastructure" |
| "I installed Docker" | "I deployed containerized applications" |

---

## 7. Forbidden Territory (The Anti-Identity)

### ❌ No "Cloud-Only" Shortcuts

- If the answer is "just use an AWS S3 bucket," it doesn't belong in this
  guide unless we're building a local S3-compatible MinIO instance first

### ❌ No Toxic Elitism

- Avoid "Just RTFM" (Read The F***ing Manual)
- If the manual is bad, we write a better one

### ❌ No Stale Content

- If a tool becomes proprietary or "anti-user," it is moved to the
  **"Legacy/Deprecation"** archive
- Version timestamps on all chapters

### ❌ No "Perfect World" Assumptions

- Don't assume the reader has a ₱50,000 budget
- Don't assume they're in a metro area with fiber
- Don't assume English is their first language (define jargon, avoid idioms)

---

## 8. AI Agent Instructions

When an AI agent is asked to write or edit content:

1. **Always read this file first** before generating any content
2. **Apply the Template Structure** (Section 3) to every new chapter
3. **Include at least one inline callout** per major section
4. **End with "Career Boost"** section that includes both bullet points AND
   interview talking point
5. **Include "Stress Test" section** in every chapter
6. **Include "PH Context" section** in every chapter
7. **Flag potential issues**: If something might break on older hardware,
   add a ⚠️ or 💸 callout
8. **Question assumptions**: If a tech choice seems arbitrary, ask
   "Why this over that?"

### Example AI Prompt Template

```
"Write chapter N on [TOPIC] following the template structure defined in
agents.md.
Include: The Story, Quick Start, The Why, Deep Dive, Career Boost, PH Context,
Stress Test.
Target audience: Filipino tech professionals/beginners with
₱0-₱30,000 hardware budgets.
Tone: Empathetic but candid, opinionated, witty, anti-gatekeeping."
```

---

## 9. Chapter Template (For New Content)

```markdown
# Chapter N: [Title]

## What You'll Build
[One-paragraph description of the outcome]

## How Long It Takes
[Realistic time estimate]

## What You Need
- [Prerequisites list]

---

## The Story
[Narrative hook — relatable scenario, Filipino context]

## Why This Matters
[Career and technical value]

## 🟢 Quick Start
[Hands-on steps — the "how"]

## 🔵 The Why
[Conceptual understanding — the "why"]

## 🟣 Deep Dive
[Advanced topics for those who want more]

## 💼 Career Boost
[What employers care about + interview talking point]

## 🇵🇭 PH Context
[Philippine-specific considerations: costs, ISPs, hardware, culture]

## Stress Test
[How to intentionally break the setup to prove it works]

## What's Next
[Transition to next chapter]

## Go Deeper
[Further reading links]
```

---

## 10. Living Document Protocol

This file evolves with the project:

- Changes require **consensus** (human + AI review)
- Major shifts go in a `CHANGELOG.md`
- Version this file: `agents.md v1.0`

---

**Last Updated:** 2026-05-16
**Version:** 2.0.0

---

> **📢 Remember:** This isn't a technical manual. It's a
> **manifesto and mentor**.
> Every word should either teach a skill, build confidence, or open a
> career door.
