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

- **Log** (`_Log.md`) is the one place to jot — daily record and capture in one file, newest day on top. Under each `## YYYY-MM-DD`, drop tasks, fleeting thoughts, what happened, and links to notes made that day (a dated provenance trail). It is *not* a task ledger: in the Morning Review its captures are sorted out — tasks to a project or the calendar, an idea worth developing to a `#seedling` note, the rest kept as record or deleted.
- **Projects** live in their own notes, indexed by `projects/_Projects.md`. A task tied to a project belongs in the project note, which carries the next action; pull an action into the day's log only when you intend to do it. Projects carry a status — **planned → active → done**. A planned project (committed, within ~6 months, not started) stays a line under the Planned heading until it has real scope or begins; don't create empty project notes, the same discipline as seed notes. Uncommitted possibilities live in the same note's **Someday** section — an inert park, not a queue — and graduate to Planned only when a real pull appears.
- **Calendar** holds only time-bound events — appointments and dated deadlines. Never a task store. If a thing isn't tied to a clock, it goes in the log or a project. In Google Calendar these are **events, never Google Tasks** — Tasks would recreate a competing store outside the vault.

### Morning Review
Run this in 15 minutes or less and focus only on deciding and routing. Anything you have not written down is an open loop your mind keeps holding. The review closes those loops so you can stop carrying them. Follow the order *Every Commitment Needs Tracking Today* (Email, Calendar, Notes, Tasks, Today).

1. **Email.** Process the inbox one message at a time. Starting with the oldest, ask whether it needs action. If it does, make it a task. If it links to something worth reading, send that to the reading inbox. Otherwise archive it and move on.
2. **Calendar.** Look at today's schedule and determine what needs preparation. Then look at every event since your last review and determine what follow-up you owe. Anything that turns up becomes a task.
3. **Notes.** Work through everything you have added to the log since your last review. For every idea worth developing, create a `#seedling` note. For every note you made, ask what it needs next and write that down as a task. Don't overthink the sort; the point is that a good idea survives, not that the filing is perfect.
4. **Tasks.** State each task you collected as a concrete next action. Add it to a project note if it serves a project or to the calendar if it's time-bound. Confirm every active project has a next action, and scan `#waiting` for anything stalled.
5. **Today.** Pick a maximum of three tasks, favoring ones that serve the week's priorities. Identify the one that matters most, then close the review and begin work on it.

Returning after skipping a few days? Don't process the whole pile. Shed the captures in bulk, keep only the few that still pull at you, and move on.

### Weekly Review
Once a week, run this in about 5 minutes. It covers what the daily cannot. Sweep the whole week on the calendar, back for follow-up that slipped and forward for what needs preparing. Skim `_Projects` and promote, demote, or park anything that has shifted. Clear stale `#waiting`. Glance at your active project and `_Learning` for drift. Then name the three priorities for the week, and let the daily pick tasks that serve them. Sunday evening or Monday morning suits it. This is a glance, not a note, so skip it freely.
