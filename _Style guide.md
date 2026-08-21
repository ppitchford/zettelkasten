---
id: "20260722165219"
date: 2026-07-22
---
# Style guide

The conventions this vault is written to. A note that violates one of these is a note to fix, not a new style to accommodate.
## Dates
`YYYY-MM-DD` everywhere in prose and the `date` field. The `id` is the same instant as a 14-digit timestamp, `YYYYMMDDHHmmss`.
## Titles
State the atomic claim directly — the title is the idea, not a topic label. Hedge deliberately: `may` when the claim is provisional, deterministic phrasing when it isn't. No semicolons; a title that needs one is two notes. The title is also the **filename**: a note is created with a timestamp name and renamed to its claim once the idea is fully formed. The `id` stays in frontmatter as the permanent address, so the filename can change as the wording sharpens — `iwe rename <old-key> <new-key>` updates every inbound reference.
## Atomicity
One idea per note. The body carries the mechanism without restating the title, and each sentence carries distinct weight — if a sentence only rephrases the one before it, cut it.
## Links
`—` (em dash) separates a link from its description. Descriptions are directional and relational — they say what *this* note does to the one it points at, never that the two are merely related or share a category. Link specific ideas, not categories. Leave a link empty rather than force a connection that isn't honest.
## Tags
Topic tags are pluralized — `#books`, not `#book`. Status tags are the exception: `#seedling` marks a root note filling the writing-inbox role now that the folder is retired, and it stays until the note is developed. Status describes one note, so it reads in the singular.
## Entry points
Hand-curated topic notes are the way back in — a note that gathers the zettels on one subject and states how they bear on each other. They are written, not generated: a zettel appears in one because you judged that it belongs. Tags and search are the coarse fallback, not the map. An Ango-style `categories` property driving a generated view stays deferred, and is worth adding only if a dashboard would show something curation cannot.
## Source vs. synthesis
A note body stays grounded in its single source. Original synthesis and cross-source connections live in *separate linked notes* — never imported into a source note's body. This is the rule that keeps a source note honest about where its claim came from.
## Writing
Original long-form writing is the *output* of the slip-box, a layer above the zettels: an essay links *down* into the atomic notes it draws from. Distinct from a zettel (atomic, source-grounded) and from the log (dated capture).

`writing/` was removed on 2026-08-19. It held two drafts that were neither essays nor zettels — no frontmatter, no header, no links in either direction — and they moved out of the vault with it. **Open: where finished long-form lives.** If it lives outside the vault it cannot link down into the zettels, which is the whole point of the layer; that argues for recreating `writing/` the day a real essay exists rather than for a home elsewhere.
## Sources
Source notes live in root like everything else — the `sources/` folder was removed on 2026-08-20 holding one note. A source note holds extracts from one book or article — other people's words and ideas, page-cited — and is the raw material a zettel is drawn from. It is exempt from the title convention: the filename is `Author — Title`, not an atomic claim, because the note makes no claim of its own. A source note is created only when there are extracts to type into it. 

**There is no digital staging.** Books queue as a physical pile; articles are not captured at all. The note is created at that moment and mints its `id` then.

The Web Clipper and `reading inbox/` were removed on 2026-08-19. In seven weeks the clipper produced 175 clips and one source note. Capture was frictionless, so nothing was weighed before it was kept, and the full-text artifact it left behind read as progress without being any — [[Insecurity work is dangerous because it is frictionless]] names the mechanism. **Open: what, if anything, replaces article capture.** The honest candidate is nothing.

## Frontmatter
`id` and `date` are written when the note is created. The Obsidian Templates plugin that used to scaffold them, and `templates/`, were removed on 2026-08-19; IWE carries a `document_template` and `key_template` in `.iwe/config.toml` but they are not yet wired to produce this frontmatter. **Open: scaffolding mechanism.** There is no `aliases` field — the filename *is* the title (see Titles), so links already read as prose. Titles are written by hand after the idea is fully formed — never auto-generated.
## Task system
Three surfaces hold tasks: the running log for capture, Things3 for anything open, and the calendar for anything tied to a clock. Project notes are not one of them — they hold scope, not tasks. Nothing else, and no status apparatus.

- **Log** (`_Log.md`) is the one place to jot — daily record and capture in one file, newest day on top. Under each `## YYYY-MM-DD`, drop tasks, fleeting thoughts, what happened, and links to notes made that day (a dated provenance trail). It is *not* a task ledger: its captures get sorted out on review — tasks to Things3 or the calendar, an idea worth developing into a note, the rest kept as record or deleted.
- **Things3** holds every open task and every next action. The vault does not keep a second copy: checklists and `**Next action:**` fields were stripped from the project notes on 2026-08-20, because two stores of the same task means one of them is always stale, and the vault's copy was the one nobody read. Pull an action into the day's log only when you intend to do it.
- **Projects** live in their own notes, indexed by `_Projects.md`. A project note carries scope, approach, and the reasoning behind them — why the thing is worth doing and what *done* means — not the task list. A project with a source directory carries that in the repo instead, beside the code, and appears here only as an indexed line with a path; the vault holds what would survive the repo's deletion. Projects carry a status — **planned → active → shipped** — with **Waiting** for work blocked on something outside your control. Active means in progress now, not merely intended. A planned project (committed, within ~6 months, not started) stays a line under the Planned heading until it has real scope or begins; don't create empty project notes, the same discipline as seed notes. Uncommitted possibilities live in the same note's **Someday** section — an inert park, not a queue — and graduate to Planned only when a real pull appears.
- **Calendar** holds only time-bound events — appointments and dated deadlines. Never a task store. If a thing isn't tied to a clock, it goes in the log or Things3. In Google Calendar these are **events, never Google Tasks** — Tasks would be a third task store beside Things3.
### Reviews
The daily and weekly routines are a Things3 checklist, not a vault convention — four of the five daily steps were always Email, Calendar, Tasks and Today, which happen in the app. The one step that touches the vault is in that checklist too: work through the log's captures, turn what is worth developing into a note, delete the rest.

Don't re-add the routine here. It was removed on 2026-08-20, when the task system moved out of the vault.
