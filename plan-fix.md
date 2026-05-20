# Fix Plan: Bahay Bahayan Launch Readiness

**Date:** May 19, 2026
**Source:** `docs/launch-readiness-report.md` (score: 8.1/10 — CONDITIONAL LAUNCH)
**Total estimated effort:** ~3.5 hours across 7 tasks

---

## Execution Order

Priority groups are ordered: **Must Fix → Should Fix → Nice to Have (v1.1)**. Tasks within each group can be done in parallel or sequentially.

---

## 🔴 Must Fix (Launch Blockers)

### Task 1: Rename `chapter-14.md` → `chapter-13.md`
- **File:** `docs/chapters/chapter-14.md`
- **Action:**
  - Rename file to `chapter-13.md`
  - Update `mkdocs.yml` nav entry from `chapter-14.md` to `chapter-13.md`
  - Update `commands.md` line 204: "Ch 14" → "Ch 13"
  - Update `community.md` line 5: `chapter-14.md` → `chapter-13.md`
- **Time:** ~2 min

### Task 2: Fix README.md Chapter 14 References
- **File:** `README.md`
- **Action:** Find and replace all "Chapter 14" / "Ch 14" references with "Chapter 13" / "Ch 13"
  - Line 42: chapter table entry
  - Line 72: Quick Start section
  - Line 123: directory structure showing `chapter-14-homelab-portfolio.md`
- **Time:** ~5 min

---

## 🟡 Should Fix (Polish)

### Task 3: Add PH Context Section to Chapter 1
- **File:** `docs/chapters/chapter-01.md`
- **Action:** Add a `## 🇵🇭 PH Context` section near the end of the chapter (before `## What's Next`), covering:
  - Why homelab matters in PH context (expensive cloud, local ISP issues, CGNAT)
  - Cost comparison: cloud services (₱) vs. homelab one-time investment
  - Power cost awareness (Meralco ₱10-12/kWh)
  - Local community resources
- **Time:** ~20 min

### Task 4: Rename Section Headers in Ch 2 and Ch 4 to Match Template
- **Files:** `docs/chapters/chapter-02.md`, `docs/chapters/chapter-04.md`
- **Action for Ch 2:**
  - Add `## Why This Matters` section (or rename existing section)
  - Rename `"Choose Your Path"` → `## 🟢 Quick Start`
  - Rename `"Install Ubuntu Server"` → `## 🔵 The Why` (or restructure to fit Quick Start → The Why → Deep Dive flow)
  - Add `## 🟣 Deep Dive` section or rename existing step section
- **Action for Ch 4:**
  - Add `## Why This Matters` section
  - Rename `"Choose Your Service"` → `## 🟢 Quick Start`
  - Add `## 🔵 The Why` section
  - Add `## 🟣 Deep Dive` section (currently only inline callout at line 189)
- **Note:** These chapters have the content, just under non-standard headings. This is a header rename + minor restructuring, not new content.
- **Time:** ~15 min

### Task 5: Add Stress Test to Chapter 9
- **File:** `docs/chapters/chapter-09.md`
- **Action:** Add a `## Stress Test` section before `## What's Next`, covering:
  - What happens if your backup drive fails?
  - Test restore procedure: pick a file, delete it, restore from backup
  - Verify 3-2-1 rule: 3 copies, 2 media types, 1 offsite
  - Schedule a monthly restore drill
- **Time:** ~20 min

### Task 6: Add 💸 Lean Path Callouts
- **Files:** `docs/chapters/chapter-01.md`, `chapter-03.md`, `chapter-06.md`, `chapter-07.md`, `chapter-08.md`, `chapter-09.md`, `chapter-10.md`, `chapter-11.md`, `chapter-12.md`
- **Action:** Add at least one `> 💸 Lean Path:` blockquote per chapter where a budget-conscious alternative exists:
  - Ch 1: Free cloud trials vs. old laptop
  - Ch 3: Docker Desktop (free for personal) vs. alternatives
  - Ch 6: Cheap managed switch (₱) options
  - Ch 7: Free tier domain vs. dynamic DNS
  - Ch 8: Free monitoring tools vs. paid alternatives
  - Ch 9: Free cloud backup tier vs. local drive
  - Ch 10: Free automation tools
  - Ch 11: Grafana/Prometheus (free) vs. paid monitoring
  - Ch 12: Free security tools (fail2ban, UFW)
- **Time:** ~45 min

---

## 🟢 Nice to Have (v1.1 — Not Launch Blockers)

### Task 7a: Add Taglish to chapters lacking it
- **Files:** `chapter-02.md`, `chapter-03.md`, `chapter-06.md` through `chapter-12.md`, `chapter-13.md`
- **Action:** Infuse natural Taglish into narration, transitions, and callouts in the 10 chapters that are entirely English. Follow the style of Ch 1, Ch 4, and Ch 5 as reference. Don't force Filipino — use it where it feels natural for a Filipino tech professional.
- **Scope:** Light pass — add 3-5 Taglish phrases per chapter, not full translation
- **Time:** ~1.5 hrs (10 min/chapter)

### Task 7b: Expand Glossary
- **File:** `docs/appendix/glossary.md`
- **Current:** 35 terms
- **Target:** 100+ terms
- **Action:** Add missing terms used across chapters (Docker-specific terms, networking terms, backup/restore terms, security terms)
- **Time:** ~30 min

### Task 7c: Expand Community Appendix
- **File:** `docs/appendix/community.md`
- **Current:** 59 lines, DEP only
- **Action:** Add PH Facebook groups, Discord servers, YouTube channels, local meetups
- **Time:** ~15 min

### Task 7d: Add More PH ISP References
- **Files:** `chapter-09.md`, `chapter-10.md`, `chapter-11.md`, `chapter-12.md`
- **Action:** Weave in PLDT/Globe/Converge/DITO mentions, CGNAT issues, and local ISP-specific troubleshooting tips in PH Context sections
- **Time:** ~20 min

### Task 7e: Add 🚀 Turbo Callouts
- **Files:** 11 chapters missing this callout
- **Action:** Add power-user shortcuts, advanced tips, or optional deep-dive paths
- **Time:** ~30 min

---

## Summary

| Priority | # Tasks | Est. Time | Blocks Launch? |
|----------|---------|-----------|----------------|
| 🔴 Must Fix | 2 | ~7 min | YES |
| 🟡 Should Fix | 4 | ~1 hr 40 min | NO |
| 🟢 Nice to Have | 5 | ~2 hrs 35 min | NO (v1.1) |
| **Total** | **11** | **~3.5 hrs** | |

**Minimum viable launch:** Tasks 1 + 2 only (~10 min total).
