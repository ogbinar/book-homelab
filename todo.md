# Bahay Bahayan Incorporation Plan

**Purpose:** turn the current manuscript into a coherent homelab book that keeps the existing Filipino-first identity, but gains a clearer maturity ladder, a home-shaped metaphor system, and a cleaner path for advanced topics like game hosting, Dokploy-style operations, and local hosted LLMs.

**Working rule:** keep the voice simple, slightly formal, practical, and Taglish where it feels natural. Do not turn the book into enterprise DevOps. Do not turn it into a generic self-hosting catalog. Everything should still feel like it belongs in a real Filipino home.

**Companion map:** see [chapter-map.md](/projects/book-homelab/chapter-map.md) for the chapter-by-chapter revision plan.

**Status:** the core manuscript integration is implemented, but a final publication cleanup pass is still needed. The checklist below now serves as the living backlog for that cleanup work and any future expansion.

---

## 1. Lock the new organizing model

- [ ] Confirm the primary progression model in the design docs as `Prep -> Kinder -> Elementary -> High School -> College -> Masters -> PhD -> Postdoc`.
- [ ] Keep the school ladder as a maturity signal, not a status hierarchy.
- [ ] Use the ladder to decide what belongs in the core manuscript, what belongs in deep dives, and what belongs in future expansion.
- [ ] Keep `Part I` as first wins, `Part II` as connected systems, and `Part III` as advanced architecture and experimentation.
- [ ] Preserve the current identity: warm, Filipino-first, slightly formal, anti-gatekeeping, practical, and budget-aware.

## 2. Keep the house metaphor consistent

- [ ] Treat homelab concepts as household services instead of abstract infrastructure whenever possible.
- [ ] Use `power`, `water`, `front desk`, `utility cabinet`, `smoke detector`, `locks`, `game room`, and `study room` as recurring metaphors.
- [ ] Keep the metaphor light and helpful, not forced or childish.
- [ ] Use the metaphor to help readers remember what each service is for and where it belongs in the house.
- [ ] Make sure the metaphor supports the technical explanation instead of replacing it.

## 3. Add a light stakeholder lens

- [ ] Add a small recurring lens for `self`, `family`, `public`, and `service management`.
- [ ] Keep it brief, funny, and practical.
- [ ] Use it as a check on trust, access, and maintenance, not as a full framework chapter.
- [ ] Add the lens as a short callout in the preface or early chapters.
- [ ] Reuse it sparingly in service examples where it clarifies access and responsibility.
- [ ] Keep the question set simple: who uses it, who manages it, who can see it, and who must never see it.

## 4. Update the design documents first

- [ ] Update [design/book-design.md](/projects/book-homelab/design/book-design.md) to reflect the school ladder and household-service mapping as core design principles.
- [ ] Add explicit guidance that advanced topics must stay home-shaped, not enterprise-generic.
- [ ] Keep the current chapter identity and Part I / Part II structure intact.
- [ ] Make Part III the place for deeper, optional material rather than a new book.
- [ ] Update [agents.md](/projects/book-homelab/agents.md) so future contributions follow the same framing.
- [ ] Keep the existing chapter template, Taglish guidance, and callout system unchanged unless the new hierarchy requires a small adjustment.

## 5. Reframe the current chapters without rewriting their soul

- [ ] Chapter 1: add a small note that the book will grow from `Prep` wins to higher maturity levels, while staying home-shaped.
- [ ] Chapter 2: keep the hardware buying path, but make low-power and reused hardware feel like the first sensible path, not a compromise.
- [ ] Chapter 3: keep Docker as the toolbox entry point, and make it clear that everything later depends on this basic container mental model.
- [ ] Chapter 4: use the first real service chapter to introduce the household-service idea and set up the future path for game hosting, media, and local AI.
- [ ] Chapter 5: keep reliability and brownout survival as core themes; this is where power and household continuity should start to matter more.
- [ ] Chapter 6: strengthen the network-as-plumbing analogy and keep the home network viewpoint concrete.
- [ ] Chapter 7: keep reverse proxy and remote access as a signature chapter; this is where CGNAT reality and direct vs tunneled access should stay obvious.
- [ ] Chapter 8: use this as the natural place to talk about service organization, operational control, and the difference between raw Compose and a more managed service layer.
- [ ] Chapter 9: keep backups central, but connect them to restore drills, storage planning, and the household “water tank / pantry reserve” metaphor.
- [ ] Chapter 10: keep automation grounded in household maintenance, not clever scripting for its own sake.
- [ ] Chapter 11: keep monitoring as a household alert system, not just dashboards.
- [ ] Chapter 12: keep security as “locks, gates, and house rules,” and make public exposure feel like a deliberate choice, not default behavior.
- [ ] Chapter 13: keep the portfolio capstone, but make the story read like “I built a home system I can explain and maintain,” not “I installed random tools.”

## 6. Incorporate game hosting in a home-shaped way

- [ ] Add game hosting as a legitimate homelab use case, not a novelty aside.
- [ ] Keep it tied to household use, friends, or family play, not public server management.
- [ ] Place the main reference in Part III or as a late-stage sidebar, since it needs direct access, latency awareness, and recovery thinking.
- [ ] Use it to teach why some services need direct TCP/UDP access instead of tunnels.
- [ ] Explain the real tradeoffs: CPU, RAM, storage, latency, update risk, and recovery after crashes.
- [ ] Frame the server as the `game room` or `arcade` of the house.
- [ ] Make it clear when game hosting is a good fit and when it is a distraction.

## 7. Incorporate Dokploy-style operations carefully

- [ ] Treat Dokploy as the `operations desk`, not as a new enterprise platform chapter.
- [ ] Use it to show the move from hand-run containers to managed deployment flows.
- [ ] Keep the comparison practical: raw Docker Compose, UI-first management, and deploy/rollback/log workflows.
- [ ] Place it in the later architecture part of the book or as a deep-dive sidebar under service management.
- [ ] Explain why it matters for readers who want less manual overhead once their stack grows.
- [ ] Keep the language grounded in home operations, not platform engineering buzzwords.
- [ ] Make sure Dokploy does not become the main story; it should support the story of a more organized homelab.

## 8. Incorporate local hosted LLMs as a household utility

- [ ] Keep the local LLM story private-first and home-first.
- [ ] Frame it as a study room or private assistant desk, not as cloud AI replacement hype.
- [ ] Place the core mention in the “something useful” and future-expansion sections, then deepen it in Part III.
- [ ] Explain why local hosted LLMs matter in a homelab: privacy, control, experimentation, and local utility.
- [ ] Keep hardware guidance honest: CPU-only, small models, iGPU, or GPU-based inference depending on budget.
- [ ] Tie local AI to the existing identity of useful systems, not novelty.
- [ ] Keep the discussion practical: document chat, private assistant, retrieval over personal knowledge, and local experimentation.

## 9. Keep stakeholder thinking light

- [ ] Use stakeholder language lightly, not as a full framework chapter.
- [ ] Repeat the question only when it improves clarity: who is this for, and who maintains it?
- [ ] Use it to separate private admin surfaces from family-facing services and public-facing services.
- [ ] Apply it in examples where it explains why a service should be private, shared, or exposed.
- [ ] Avoid turning it into a formal model that slows the book down.

## 10. Reorganize Part III as the advanced expansion zone

- [ ] Keep Part III clearly out of the MVP core, but define it better so the future path feels intentional.
- [ ] Use Part III for `PhD` and `Postdoc` material: game hosting, local hosted LLMs, Dokploy-style operations, higher-end automation, and deeper architecture.
- [ ] Make Part III feel like optional ascent, not a requirement for readers who only need the core book.
- [ ] Keep the `Choose Your Path` chapter as the start of specialization paths.
- [ ] Add paths for `Games`, `AI`, and `Operations` alongside the existing DevOps, Security, Media, Privacy, and IoT tracks.
- [ ] Keep Part III explicitly future-facing so it does not bloat the current manuscript.

## 11. Tighten the manuscript hierarchy

- [ ] Make the opening chapters answer “What is this?” and “Why should I care?” with very little abstraction.
- [ ] Make the middle chapters answer “How do I make this reliable and useful at home?”
- [ ] Make the later chapters answer “How do I manage, share, protect, and expand this responsibly?”
- [ ] Reserve the advanced architecture topics for the places where the reader already has enough context to benefit from them.
- [ ] Avoid introducing too many new metaphors or side systems if they do not support the core spine.
- [ ] Keep the chapter flow readable even if someone skips the optional sections.

## 12. Update the sectioning and references

- [ ] Review chapter headings and section labels so they reflect the new hierarchy without losing the existing content.
- [ ] Add small cross-references where a later chapter explains an idea that was introduced earlier as a simple win.
- [ ] Ensure the preface explains the school ladder and household metaphor in one short, readable pass.
- [ ] Ensure the index, glossary, and appendix references still point readers to the right places.
- [ ] Keep the table of contents simple enough that a reader can see the journey at a glance.

## 13. Add the right callouts in the right places

- [ ] Add one light stakeholder note where it clarifies trust and access.
- [ ] Keep `💸 Lean Path` visible for budget-sensitive home setups.
- [ ] Keep `📢 Jargon Alert` where the new concepts are introduced.
- [ ] Use `⚠️ Watch Out` where power, exposure, or recovery can go wrong.
- [ ] Use `💡 Quick Win` where a small household-level improvement is available.
- [ ] Do not overuse `🚀 Turbo`; keep it for genuinely higher-end paths.

## 14. Preserve the Filipino identity

- [ ] Keep the book Filipino-first, not a translated import.
- [ ] Keep Taglish natural, not evenly distributed by force.
- [ ] Keep Philippine reality visible: brownouts, heat, noise, CGNAT, family sharing, budget pressure, reused hardware, and local stores.
- [ ] Keep costs in `₱` and explanations grounded in local buying patterns.
- [ ] Keep the tone slightly formal and complete where it matters, especially in preface, closing note, and structural docs.
- [ ] Keep the humor subtle and helpful, not noisy.

## 15. Update the appendices to support the new framing

- [ ] Hardware appendix: emphasize low-power compute, thermal/noise realities, brownout readiness, and home placement.
- [ ] Services appendix: reorganize by household use and trust level where useful, not just by tech category.
- [ ] Errors appendix: keep errors searchable and frame fixes in home-context terms when possible.
- [ ] Community appendix: keep it useful as a reader support layer, not a marketing list.
- [ ] Glossary: add terms that support the new house metaphor, service management, remote access, game hosting, and local AI.

## 16. Make the advanced themes feel earned

- [ ] Do not move every advanced topic into the main line of the book.
- [ ] Promote only the themes that feel distinctively homelab and clearly useful in a Filipino home.
- [ ] Keep `power resilience`, `remote access`, `storage`, `low-power compute`, `home automation`, `thermals/noise`, and `network segmentation` as the strongest core advanced themes.
- [ ] Keep `game hosting`, `Dokploy-style operations`, and `local hosted LLMs` visible, but placed later or deeper.
- [ ] Demote anything that turns into generic enterprise practice unless it is tightly framed around the home.

## 17. Validation checklist

- [x] A first-time reader can still see a clear path from zero to useful homelab.
- [x] The new hierarchy does not break the current manuscript tone.
- [x] The house metaphor helps memory without feeling overdesigned.
- [x] The stakeholder lens stays light and useful.
- [x] The advanced topics feel like natural growth, not a separate book.
- [x] Game hosting, Dokploy-style operations, and local hosted LLMs are incorporated without diluting the core story.
- [x] The final book still feels like `Bahay Bahayan`: practical, home-shaped, Filipino, and worth maintaining.

## 18. Publication Cleanup Pass

This section captured the specific blockers identified in the end-to-end audit. The items below were completed in the current publication cleanup pass and are kept here as an audit trail.

- [x] Fix the broken Chapter 13 asset reference in `docs/chapters/chapter-13.md`.
  - Replace the missing `diagrams/architecture.png` reference with an actual asset path, or remove the image if the book should not depend on a diagram that does not exist yet.
  - If a new diagram is added, make sure the file exists in the repo and the relative path matches the chapter location.
  - Re-run a link/image validation after the change so Chapter 13 does not contain a dead asset reference.

- [x] Resolve the public license mismatch.
  - Choose one license and propagate it consistently across `README.md`, `docs/index.md`, `docs/appendix/author.md`, `docs/appendix/community.md`, `design/book-design.md`, and any other public-facing references.
  - Keep the prose/docs license and code-snippet license clearly separated if both are meant to exist.
  - Make sure the repository front door, appendix pages, and design docs all tell the same licensing story.
  - If the book stays under `CC BY-SA 4.0`, update the README top matter and any stale `CC BY 4.0` references.
  - If the book moves to `CC BY 4.0`, update the appendix/footer/license snippets that still say `CC BY-SA 4.0`.

- [x] Fix the public GitHub repository link and related metadata.
  - Update `docs/index.md` so the repo link points to the actual public repository.
  - Scan other front-facing pages for stale repo URLs and make them consistent.
  - Verify the author and site metadata do not point to different identities or old locations.
  - Make the repo link in the book front door match the README exactly.
  - Make sure the social/link metadata in the front door does not point at a placeholder or stale project fork.

- [x] Normalize chapter naming and structure drift.
  - Decide on one canonical title for Chapter 1 and use it consistently across the file, index, part pages, and navigation.
  - Review chapter headings in Chapters 1, 2, 4, 11, and 13 so they better match the declared `agents.md` structure without forcing awkward template compliance.
  - Remove duplicate structural markers such as repeated `What's Next` blocks or duplicated subheadings.
  - Keep the chapter flow readable even when the book intentionally bends the template.
  - Chapter 1 should use one consistent book-facing title everywhere, not a mix of `What Even Is a Homelab?` and `Ano ba ang Homelab?`
  - Chapter 4 should remove any duplicated subsection heading text and preserve only one clear `Vaultwarden` heading.
  - Chapter 13 should keep one `What's Next` section in the main chapter flow and avoid accidental repetition inside templates.
  - Chapter 11 should either add a proper `Deep Dive` or clearly mark the current structure as intentional in the style guide.

- [x] Clean up the Part I / Part II landing pages.
  - Decide whether `docs/part-i.md` and `docs/part-ii.md` are true reading milestones or mostly summary pages.
  - If they are summary pages, keep them concise and clearly secondary to the chapter flow.
  - If they are meant to be part of the reading experience, make their transitions and references feel less like side doors.
  - Keep the part pages useful for orientation, but do not let them feel like mandatory duplicate reading before the chapters.
  - If the left-nav path stays dominant, simplify the part-page prose so it reads like a gentle bridge rather than a second introduction.

- [x] Reduce stale or duplicated planning artifacts.
  - Decide which of `todo.md`, `chapter-map.md`, `design/book-design.md`, and `agents.md` is the canonical place for ongoing structure decisions.
  - Archive or collapse redundant planning language so the repo does not present four slightly different versions of the same plan.
  - Keep one source of truth for future chapter restructuring, and make the other docs point back to it.
  - If `todo.md` is the live backlog, keep it action-oriented and move completed plan language into the archive documents.
  - If `chapter-map.md` is the live revision map, trim duplicated strategy language out of `todo.md` so the two files do not compete.
  - Make sure `design/book-design.md` stays the canonical high-level rules doc, while `agents.md` stays the contribution/style guide.

- [x] Clean the community appendix so it serves the reader first.
  - Soften any DEP-first framing that makes the page feel like a brand page instead of a reader resource.
  - Keep the appendix broad enough to help a beginner find communities, meetups, and learning channels without implying a single community is the only path.
  - Make sure the community appendix matches the anti-gatekeeping tone of the rest of the book.
  - Reword the opening so the reader sees options, not a command to join one specific community first.
  - Keep DEP as an important example, but not as the only door into the hobby.

- [x] Tighten the glossary and appendix metadata.
  - Confirm glossary ordering and naming consistency so the page feels like a deliberate reference, not a rolling scratchpad.
  - Check appendix cross-references and chapter references so they point to the chapter readers actually use first.
  - Remove or update any appendix labels that still sound like draft language.
  - If a term appears in the book often, make sure the glossary has a clean definition and a stable cross-reference.
  - If an appendix is archived or historical, label it clearly so it does not read like active reading material.

- [x] Perform a final tone pass on the remaining outliers.
  - Re-read Chapter 7 and Chapter 13 for any overly dramatic, employer-first, or performance-oriented lines.
  - Re-check Chapter 11 and Chapter 12 for security/monitoring language that still reads too enterprise-heavy.
  - Keep the calm kuya voice, practical Taglish, and home-shaped framing intact all the way through the final pages.
  - Make sure any “chaos/challenge” callouts still sound like helpful stress tests, not dare-based theater.
  - Remove or soften lines that read like recruiter bait if they are not serving the home-first reading experience.

- [x] Clean up orphan and archive pages so the public front door feels intentional.
  - Treat `docs/launch-readiness-report.md` as an archive page only, not a live part of the reading path.
  - Decide whether `CHANGELOG.md`, `review-report.md`, `plan-fix.md`, and `plan-fix-review.md` should stay visible in the repo root or move into an archive/docs folder.
  - Keep `chapter-map.md` visible only if it is still the active source of structure decisions; otherwise mark it as archival or fold it into `todo.md`.
  - If the README directory tree keeps the planning artifacts, clearly label them as internal or archived so the public-facing book does not feel half-finished.
