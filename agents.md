# 📜 Agents.md: Bahay Bahayan Style Guide

This document defines the identity, voice, and structural integrity of the
**Book Homelab** guide. It keeps the manuscript aligned around one voice:
a practical Filipino kuya guiding the reader from first wins to responsible
home-shaped systems. All contributors and AI agents assisting in this work
must follow these rules.

---

## 1. The Core Identity: "The Guiding Kuya"

### The Persona

A seasoned homelab builder who speaks like a practical older sibling:
calm, helpful, slightly formal when needed, and never trying to impress.
We value the feeling of a setup that works quietly at home, the satisfaction
of a clean `systemctl status`, and the confidence that comes from knowing what
to do next.

### The Mission

To help Filipino readers move from tech consumer to responsible builder of a
home lab that is practical, affordable, private, and actually useful in a
real household.

### The Framing

- Use a school ladder to signal maturity: `Prep`, `Kinder`, `Elementary`, `High School`, `College`, `Masters`, `PhD`, `Postdoc`
- Keep examples rooted in the household: power, water, front desk, utility cabinet, game room, study room, locks, alerts, and house rules
- Treat advanced topics as earned depth, not as enterprise cosplay
- Keep career value present, but secondary to home usefulness
- Use `Bahala Na` only carefully, and only when it means brave action with preparation

### The Promise

We don't just provide copy-paste commands; we provide the intuition behind
the setup. Every `docker run` comes with a reason. Every network decision
comes with a plain-language explanation. Every chapter should leave the
reader calmer, not more confused.

---

## 2. The Voice & Tone (Personality)

### Calm, Practical, and Human

- We acknowledge that networking is hard and DNS is often the problem.
- We validate frustration without dramatizing it.
- We explain the mistake, the fix, and the lesson.
- Example: *"Oo, papalya rin 'yan minsan. Here's what usually broke for me."*

### Opinionated, But Not Performative

- If a tool is bloated or unnecessary for a home setup, say so plainly.
- Prefer simple, maintainable options over fashionable ones.
- Explain why a choice fits a Filipino home, not just a technical benchmark.
- Example: *"We use Docker Compose here because it is enough for one home, one admin, and one clear recovery path."*

### Warm and Grounded

- Use analogies from the physical world and from the house.
- Keep humor light and familiar.
- Avoid corporate-speak and enterprise inflation.
- Example: *"Think of your reverse proxy like the front desk of the house: it routes visitors, but it doesn't hand out every key."*

### Anti-Gatekeeping

- We speak to the student, the parent, the hobbyist, and the career shifter.
- If a term is jargon, define it once with a `Jargon Alert` and then use it normally.
- Example: *"Jargon Alert: 'Idempotent' just means running the command many times gives the same result."*
- Never shame the reader for starting small.

### Taglish Guidance

- Use Taglish when it sounds natural in a real Filipino conversation.
- Keep it light and readable; do not force a bilingual line just to be clever.
- Put the most natural Taglish in the preface, opening chapters, closing note, and a few lived-in examples.
- Keep appendices clearer and more reference-like, with only light Filipino texture.

---

## 3. Structural Rules (The "Lab" Framework)

Every chapter or module should follow the template below closely enough to
feel like the same book, but not so rigidly that the voice becomes mechanical.
The structure is optimized for Filipino beginners: story first, then useful
technical depth.

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
[Optional or brief: how this can help at work, in interviews, or in a portfolio]

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
| Career Boost | ⚠️ Keep brief | Use only when it helps the reader; do not let it dominate the chapter |
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

## 6. The "Career Value" Rule

Every technical tutorial may answer:
**"How could this help at work or in a portfolio?"**

### Career Boost Section Format

At the end of a chapter, include a **Career Boost** section only if it
genuinely helps the reader see the value outside the home.

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

(Use this template only when it fits the chapter. Keep it brief.)
```

### Optional Translation Table

When appropriate, include a small translation table showing how homelab terms
map to professional language. Do not force one into every chapter, and keep it
brief when you do use it.

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
3. **Include at least one inline callout** per major section where it helps
4. **Use "Career Boost" only when it adds value; keep it brief**
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
[Brief career value, only when relevant]

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
> **guide and mentor**.
> Every word should either teach a skill, build confidence, or help the
> reader use something useful at home.
