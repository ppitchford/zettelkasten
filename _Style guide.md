---
id: "20260722165219"
date: 2026-07-22
aliases:
  - Style guide
---
# Style guide

The conventions this vault is written to. A note that violates one of these is a note to fix, not a new style to accommodate.

## Dates
`YYYY-MM-DD` everywhere in prose and the `date` field. The `id` is the same instant as a 14-digit timestamp, `YYYYMMDDHHmmss`.

## Titles
State the atomic claim directly — the title is the idea, not a topic label. Hedge deliberately: `may` when the claim is provisional, deterministic phrasing when it isn't. No semicolons; a title that needs one is two notes. The title is also the **filename**: a note is created with a timestamp name and renamed to its claim once the idea is fully formed. The `id` stays in frontmatter as the permanent address, so the filename can change as the wording sharpens — Obsidian updates the links.

## Atomicity
One idea per note. The body carries the mechanism without restating the title, and each sentence carries distinct weight — if a sentence only rephrases the one before it, cut it.

## Links
`—` (em dash) separates a link from its description. Descriptions are directional and relational — they say what *this* note does to the one it points at, never that the two are merely related or share a category. Link specific ideas, not categories. Leave a link empty rather than force a connection that isn't honest.

## Tags
Topic tags are pluralized — `#books`, not `#book`. Status tags are the exception: `#seedling` marks a root note filling the writing-inbox role now that the folder is retired, and it stays until the note is developed. Status describes one note, so it reads in the singular.

## Source vs. synthesis
A note body stays grounded in its single source. Original synthesis and cross-source connections live in *separate linked notes* — never imported into a source note's body. This is the rule that keeps a source note honest about where its claim came from.

## Writing
Original long-form writing — essays, drafts, reflections — lives in `writing/`, not root. It is the *output* of the slip-box, a layer above the zettels: an essay links *down* into the atomic notes it draws from. Distinct from a zettel (atomic, source-grounded) and from the log (dated capture).

## Frontmatter
`id` and `date` are scaffolded by the Unique note creator from `templates/note`. There is no `aliases` field — the filename *is* the title (see Titles), so links already read as prose. Titles are written by hand after the idea is fully formed — never auto-generated.

## Task system
Three surfaces: a single running log, project notes, and the calendar. Nothing else — no separate task inbox, no status apparatus.

- **Log** (`_Log.md`) is the one place to jot — daily record and capture in one file, newest day on top. Under each `## YYYY-MM-DD`, drop tasks, fleeting thoughts, what happened, and links to notes made that day (a dated provenance trail). It is *not* a task ledger: in the Morning Review its captures are sorted out — tasks to a project or the calendar, a thought with legs to a `#seedling` note, the rest kept as record or deleted.
- **Projects** live in their own notes, indexed by `projects/_Projects.md`. A task tied to a project belongs in the project note, which carries the next action; pull an action into the day's log only when you intend to do it. Projects carry a status — **planned → active → done**. A planned project (committed, within ~6 months, not started) stays a line under the Planned heading until it has real scope or begins; don't create empty project notes, the same discipline as seed notes. Uncommitted possibilities live in the same note's **Someday** section — an inert park, not a queue — and graduate to Planned only when a real pull appears.
- **Calendar** holds only time-bound events — appointments and dated deadlines. Never a task store. If a thing isn't tied to a clock, it goes in the log or a project. In Google Calendar these are **events, never Google Tasks** — Tasks would recreate a competing store outside the vault.

The system is run by a daily **Morning Review** — a ~15-minute *triage, not a work session*: it only decides and routes; execution comes after.

1. Skim the calendar for what's ahead and anything to prep.
2. Process the Gmail inbox, routing each item to a task, a clip, or a calendar event.
3. Sort the top of the log — send each capture to its home (a project, the calendar, a `#seedling` note, or the trash).
4. Confirm each active project carries a next action.
5. Scan `#waiting` and nudge anything that has gone quiet.
6. Set today under the day's heading: at most ~3 tasks, and **star the one** that would make the day a win. Send anything time-bound to the calendar.

After skipped days, don't guilt-process the pile — bulk-shed the captures per the lossy rule, keep the few that still pull, and move on.

A lighter **weekly review** (optional, ~5 minutes, skippable) covers what the daily can't: skim `_Projects` (promote / demote / park anything?), clear stale `#waiting`, and glance at the active project and `_Learning` for drift. A weekly glance, not a weekly note.
