# TODO — Zettelkasten Restructure & Things3 Migration

## Goal
Restructure the Obsidian vault around a flattened layout, a weekly-note stream that replaces the journal, a reading inbox, project notes with an index, and a Someday park — then migrate the pruned Things3 backlog into it.

## Tasks

### 1. Conventions & structure (do first — everything else depends on it) — ✅ done
- [x] Write a **style-guide note** consolidating existing conventions: `YYYY-MM-DD` dates, `—` em-dash link separator, atomic-claim titles (no semicolons, deliberate hedging), pluralized tags, source-vs-synthesis rule, "link ideas not categories."
- [x] Define the **flattened layout**: own notes (zettels, seed notes, topic/MOC notes) move to **root**; keep `templates/`; add `attachments/` (admin); retire `journal/` and the `inbox/` *folder*.
- [x] Flatten `notes/`, `inbox/`, `topics/` into root; the writing-inbox role becomes root notes tagged `#seedling` until developed. *(no `topics/` existed; journal notes moved to root rather than deleted.)*
- [x] Update path-qualified internal links to bare wikilinks (batch script — reuse the alias-injection approach); verify no broken links after the move. *(also converted 9 Markdown-style links to wikilinks; 39 links verified, 0 broken.)*
- [x] Convert template frontmatter from `zk` `{{id}}`/`{{date}}` placeholders to Obsidian Templates syntax; retires the carried-over YAML warnings. *(dropped templater for the core Unique note creator; ids quoted so they register as text.)*

### 2. Task system — ✅ done
- [x] Create a **weekly-note template**: plan section up top, optional `## YYYY-MM-DD` daily subheadings for the inline record; undone items roll forward or drop. *(`templates/weekly.md`; notes live in `log/`, ISO-week named, created via core Templates; created notes linked under their day for provenance.)*
- [x] Retire `journal/`: stop generating daily journal notes; the weekly note is now both plan and record. *(done in Phase 1: daily-notes disabled, `daily-notes.json` cleared, `journal/` removed.)*
- [x] Establish the **calendar** as a thin appointment layer only (time-bound events, not a task store). *(documented as the Task system section in `Style guide.md`.)*

### 3. Reading inbox — ✅ core done (3.3 deferred)
- [x] Create the **reading-inbox note** with an explicit header stating it is *lossy* — skim, pull what still compels, let the rest fall away without guilt. *(`Reading inbox.md`.)*
- [x] Populate it with the routed backlog items (see Appendix A). *(29 items, plain bullets; dups collapsed — also settles the Phase 6 dedup line.)*
- [ ] (Optional) Adopt the Obsidian **Web Clipper** for future capture straight into the inbox. *(deferred — install/config when wanted.)*

### 4. Projects — ✅ done
- [x] Create **`Projects.md`** index listing active projects, each linking to its note.
- [x] Create project note: **`frame`** (screenshot tool). *(Points to the living `~/projects/frame/ROADMAP.md` as source of truth — Tier 1 shipped, Tier 2 gate open — rather than copying it. The Appendix B "15 items" list was stale.)*
- [x] Create project note: **Personal Website** — tasks: revisit roadmap in Claude, implement Plausible analytics.
- [x] Create project note: **ArtSpacesSoCal** — task: revisit roadmap in Claude, sync tasks to actual roadmap.
- [x] Create project note: **Medicine-cabinet install** — ordered next action surfaced (see Appendix B).
- [x] Create project note: **Kitchen shelf** — ordered next action surfaced (see Appendix B).
- [x] Create project note: **Desktop "made for one"** (proof-of-concept) — next action; link to its seed note. *(Seed note is Phase 5; forward-linked as `[[Desktop made for one — seed]]`.)*

### 5. Someday park, seed notes & misc routing — mostly done
- [x] Create **`Someday.md`** park (inert, promotable; skimmed on random-revisit, not a queue); populate from Appendix C.
- [x] Create seed note: **real-time translation device** (triangular, dual e-ink). *(shell only — develop collaboratively, below.)*
- [x] Create seed note: **Desktop "made for one"** (idea side); link to its project note. *(shell only — develop collaboratively, below.)*
- [x] Create reference note: **grimdark paints/technique**. *(source-pointer to the Bene_Crafts Patreon; guides are paywalled, so distill technique when accessed.)*
- [x] (Optional) Create recipe note: **Santa Maria seasoning**. *(full recipe + substitution + references.)*
- [x] **Route the actionable items** (from the previous Someday list):
  - Pay IRS penalty → calendar (manual — deadline).
  - Juniper kit pickup → already completed.
  - Valve estimates (×3) → calendar, future date (manual — a 2-year-horizon improvement).
  - Six Types of Working Genius → Reading inbox.
  - bene_crafts Patreon → already captured in the grimdark note.
  - Spilhaus print → Someday (office, post-remodel).
  - Cog-vs-replicate decision → calendar (manual — conversation scheduled tomorrow).
  - Find a Mindful alternative → revisit in ~a month (calendar reminder, manual).
- [ ] Add to **calendar**: Maeve builds (crib, mobile arm, bookcase) against their target date. *(manual — external calendar.)*

**Collaborative follow-ups (one at a time, interactive):**
- [ ] Develop the **translation-device** seed into a real note.
- [ ] Develop the **Desktop made-for-one** seed into a real note.
- [ ] Distill **grimdark technique** from the Bene_Crafts guides into its reference note.

### 6. Backlog cleanup & verification
- [ ] Do **not** migrate: "genuine joy…" (already written), "Monotasking" (already drafted).
- [ ] Collapse duplicates to one reading-inbox entry each: "The peril of laziness lost", "You Can't Tell People Anything" (= Habitat Chronicles).
- [ ] Final pass: confirm every Things3 item is accounted for and Things3 lists can be archived.

## Notes
- **Reading inbox is lossy by design.** ~three dozen items is fine *if* it's a skim-and-shed capture, not a must-finish debt ledger. That framing is the whole safeguard.
- **Seed-note discipline:** promote a parked item to a seed note only when there's a real thought to write — an empty seed note is the same anti-pattern as a forced link.
- **Source vs. synthesis** stays enforced: zettel bodies grounded in the source; cross-source synthesis lives in separate linked notes.
- **Categories deferred:** hand-curated topic/MOC notes remain the primary associative entry points; add an Ango-style `categories` property + Bases later only if a dashboard view is wanted.
- **Task model:** one weekly-note stream (plan → record, replacing the journal) + project notes with `Projects.md` index + calendar for time-bound events. Weekly creation cadence chosen deliberately over a nightly ritual to fit an on-and-off habit.
- **Division of labor:** Philipp handles file creation, `git mv`, indexing, and commits; Claude supplies clean paste-ready content one note at a time, drafted → critiqued → revised before moving on.
- **Open detail:** whether weekly notes live in root or a small `log/` admin folder — default `log/`, adjustable.

---

## Appendix A — Reading inbox (routed from Things3)
- Separate Writing and Formatting
- How to Record and Retrieve Anything You've Ever Had to Look Up Twice
- How to effectively write quality code with AI
- Thin Desires Are Eating Your Life
- Amazon's Ring and Google's Nest — U.S. Surveillance State
- A mini data center
- How I Learned to Read Way, Way More
- Cognitive Debt: When Velocity Exceeds Comprehension
- uv is the best thing to happen to the Python ecosystem in a decade
- Rob Pike's 5 Rules of Programming
- The peril of laziness lost *(dedup)*
- Meetings are forcing functions
- The Angine de Poitrine Argument for UBI
- The Git Commands I Run Before Reading Any Code
- June thoughts *(newsletter)*
- How to help someone use a computer
- You Can't Tell People Anything / Habitat Chronicles *(dedup)*
- agents need control flow, not more prompts
- Nobody Gets Promoted for Simplicity
- Good software knows when to stop
- Media vita in morte sumus
- The Creative Power of Walking
- Using LLMs at Oxide *(also links → Personal Website / job-search toolkit)*
- Bullshit Jobs — Graeber's theory
- Atomic Design
- How to Set up a Local Coding Agent on macOS — Kyle Howells
- Master Claude Code: Proven Daily Workflows from 3 Technical Founders *(video)*
- Set up Claude Code for maximum productivity
- Use AI agents like employees

## Appendix B — Active project internals
**frame (screenshot tool):**
- *Tier 1 (v1):* op-list architecture w/ undo-redo pointer; crop tool; session persistence (save/reopen preserves op list, cross-session undo); self-timer w/ configurable delay; last-N thumbnails in overlay; previous-capture quick recall; local capture-history (last N on disk); sidecar JSON annotation format; replace `~/.local/bin/screenshot` wrapper w/ `frame`; MangoWM keybindings; Rosé Pine theming (dark/light, `~/.config/theme/`); verify single-static-binary build on Void; end-to-end verification across v1 workflows.
- *Tier 2 — Candidates (Post-v1):* (to be sorted from above during roadmap sync)
- *Tier 3 — Parked:* (as flagged)

**Medicine-cabinet install (ordered):** clear-coat frames/shelf pieces → drill side pieces for brackets → install frames into cavities → install shelf brackets → install shelves → joint-compound the gaps → paint the filler.

**Kitchen shelf (ordered):** round corner → sand → drill mounting holes → stain → install railing → prep walls (mark studs/placement) → install brackets → mount.

## Appendix C — Someday park (inert, promotable)
- Learning plans (7): Algorithms & Data Structures, Learn Go the Hard Way, Learn Python the Hard Way, Little Book of Linear Algebra, The Little Learner, Nature of Code, Missing Semester.
- Ventures: establish a consulting practice; get Reptidaddy going; set up GuppyLM (model training); build your own UPS.
- Aspirational builds: open-source robot vacuum; fault-tolerant RL octocopter.
- Woodworking: Small-Shop Workbench (Scott Walsh); Miter Saw Station (MWA Woodworks); mini paint holder; dry-brush texture palette; California Juniper starter kit (setup, post-arrival).
- Warhammer: assemble Inquisitor Ostromandeus; assemble Ravener Kill Team.
- Home: smart-home replacement (doorbell/lock/cameras/thermostat).
