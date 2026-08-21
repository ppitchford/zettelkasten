---
id: "20260722165219"
date: 2026-07-22
---
# Conventions

The conventions this vault is written to. A note that violates one of these is a note to fix, not a new style to accommodate.

## What this is
A Zettelkasten: atomic claims, densely linked, each grounded in its source. Developing insight is the point, not note count.

Everything that is *open* — tasks, next actions, the list of projects, appointments — is held outside the vault, in the task manager and the calendar. Two stores of the same task means one of them is always stale, and the vault's copy is the one nobody reads. What lives here is the thinking that stays worth reading after the work is finished.

The task manager is Things3, named once here and called *the task manager* everywhere below, so replacing it is a one-line edit rather than a sweep.

## The shape of the vault
Root is flat and holds every document. Links are the structure, not folders — and under IWE a document's path *is* its key, so a folder addresses rather than organizes. `projects/`, `sources/`, `reading inbox/`, `writing/`, `templates/` and `attachments/` were all removed for holding one or two files each. Don't propose topic folders, including a return of those.

The filename is the title and the `id` in frontmatter is the permanent address, so a note can be renamed as its wording sharpens without breaking anything that points at it. Links resolve by bare name, which works only while every filename in the vault is unique.

This file is the only document in the vault that isn't a note. It takes a plain name like every other file and sorts in among the zettels, and it is not a zettel, so the atomic-claim title rule doesn't apply to it.

## Writing a note
**Titles.** State the atomic claim directly — the title is the idea, not a topic label. Hedge deliberately: `may` when the claim is provisional, deterministic phrasing when it isn't. No semicolons; a title that needs one is two notes. A note is created with a timestamp name and renamed to its claim once the idea is fully formed. Titles are written by hand, never auto-generated.

**Atomicity.** One idea per note. The body carries the mechanism without restating the title, and each sentence carries distinct weight — if a sentence only rephrases the one before it, cut it.

**Frontmatter.** `id` and `date` are written when the note is created. `date` is `YYYY-MM-DD`, as dates are everywhere in prose; `id` is the same instant as a 14-digit `YYYYMMDDHHmmss` timestamp. There is no `aliases` field — the filename *is* the title, so links already read as prose.

**Type.** `type` in frontmatter names what kind of document a note is, and it is written only when the note is *not* an ordinary zettel. `type: seedling` is a rough capture — a fleeting idea, not yet worked out — and taking the line off is the event that marks the note as developed. `type: essay` is long-form output. A finished zettel carries no `type` at all: it is the default, and thirty files declaring themselves evergreen would be noise rather than information. Source notes need no type either, since `Author — Title` marks them on sight.

Hashtags were dropped on 2026-08-21. One syntax was carrying three jobs — topic, status, and kind — and `iwe find --filter` reads frontmatter, not tags.

**Source vs. synthesis.** A note body stays grounded in its single source. Original synthesis and cross-source connections live in *separate linked notes*, never imported into a source note's body. This is the rule that keeps a source note honest about where its claim came from.

## Links and entry points
`—` (em dash) separates a link from its description. Descriptions are directional and relational — they say what *this* note does to the one it points at, never that the two are merely related or share a category. Link specific ideas, not categories. Leave a link out rather than force a connection that isn't honest.

Hand-curated topic notes are the way back in: a note that gathers the zettels on one subject and states how they bear on each other. They are written, not generated — a zettel appears in one because you judged that it belongs. Tags and search are the coarse fallback, not the map. An Ango-style `categories` property driving a generated view stays deferred, and is worth adding only if a dashboard would show something curation cannot.

## Sources and reading
Source notes live in root like everything else. A source note holds extracts from one book or article — other people's words and ideas, page-cited — and is the raw material a zettel is drawn from. It is exempt from the title convention: the filename is `Author — Title`, not an atomic claim, because the note makes no claim of its own. One is created only when there are extracts to type into it.

**There is no digital staging.** Books queue as a physical pile; articles are not captured at all. The note is created at that moment and mints its `id` then.

**An observation is a source.** A claim can be grounded in something you measured rather than something you read — a command and its output, a count of a room, a rate over a period. What makes it a source is not that someone wrote it down but that the claim can be checked against something outside your memory of forming it, and a reproducible command is a stronger citation than a page number, because anyone can re-run it. Cite what was measured, how, and *when*: measured sources change while a text does not, so `wc -c *.md` run on a date and *ten of twelve patrons on their phones, 2026-08-31 18:00* both carry a timestamp that a page citation doesn't need. The claim the note makes is the mechanism; the measurement is only its evidence, and evidence is dated.

The working practice is narrower than the apparatus suggests: read, form a claim, write the zettel, cite the source inside it, and skip the source note entirely. 21 of 28 zettels already carried their own `## References` before the rule was written down.

## Long-form writing
Original long-form writing is the *output* of the slip-box, a layer above the zettels: an essay links *down* into the atomic notes it draws from. Distinct from a zettel (atomic, source-grounded) and from the log (dated capture).

## Capture, and the log
Neither lives in the vault any more, as of 2026-08-21. Capture is `~/inbox.md`, written by `inbox` from any shell and flushed into the task manager by hand; it is emptied, not kept. The dated record is `~/log/log.md` in its own private repo, because it carries medical results, job applications and money, and those don't belong beside notes that may one day be published. The conventions governing it moved with it, into that repo's `README.md`.

What the vault keeps from the arrangement is the rule that made it work: **a claim is first noticed in the log and does not live there.** When a day's entry carries a finding worth generalizing, it becomes a zettel or it goes inert. That is how the log came to outweigh the zettels 1.47 to 1 before anyone looked.

## Work that isn't in the vault
Three surfaces hold work, and none of them is a note or in the vault. `~/inbox.md` captures, the task manager holds anything open, the calendar holds anything tied to a clock.

- **The task manager** holds every open task and every next action, and the vault keeps no second copy. Pull an action back out into the day only when you intend to do it. The project ladder lives there too — **planned → active → shipped**, with **Waiting** for work blocked on something outside your control and an inert **Someday** park below all of it, which is a park and not a queue and graduates to Planned only when a real pull appears. Active means in progress now, not merely intended: a project untouched for a month belongs in Planned, and saying so is the point of the list.
- **Projects** live in their own notes, or in their repo when they have one — the repo copy stays beside the code and cannot drift out of sight, and the vault holds what would survive the repo's deletion. A project note carries scope, approach, and the reasoning behind them — why the thing is worth doing and what *done* means — never the task list. Don't create an empty project note; a project earns one when it has real scope, the same discipline as seed notes.
- **The calendar** holds only time-bound events — appointments and dated deadlines. Never a task store. In Google Calendar these are **events, never Google Tasks**, which would be a second task store beside the task manager.
- **Reviews** are a checklist in the task manager, not a vault convention: four of the five daily steps were always Email, Calendar, Tasks and Today, which happen in the app. The one step that touches the vault is in that checklist too — work through the inbox and the day's entries, turn what is worth developing into a note, delete the rest. Don't re-add the routine here.

## Conventions in waiting
- **Long-form stays in root, marked `type: essay`.** An essay links *down* into the zettels, so it has to share their namespace; publishing selects it by query — `iwe find --filter 'type: essay'` — rather than by directory. No `writing/` or `essays/` folder: the reason a folder felt necessary was publishing, and publishing is a selection problem, not a location one.
- **Article capture is Instapaper, outside the vault.** Articles are saved and read there, with the reading tracked as a task; what reaches the vault is a claim, never a clipping. If the queue starts growing faster than the notes drawn from it, drop the tool and go without one — the stopping condition is pre-committed for the reason [[Pre-commit your stopping conditions before the pressure arrives]] gives.
- **Frontmatter is scaffolded by `new-note`** in `.zshrc`, which calls `iwe create --template default` and supplies `id` and `date` itself. IWE's `{{id}}` renders a random slug rather than a timestamp, so the 14-digit `id` has to come from the shell; the template owns the body.

## What was removed, and why
The vault is small on purpose, and each of these was deliberate. The criterion throughout: what stays is what links — applied at review, not at creation. A new note may link to nothing yet; a claim can open a cluster, and `type: seedling` marks it until it does. What the criterion catches is the note that has had time and still connects to nothing in either direction, and even then the first move is to write the note that would connect it. An index manufactures links, so appearing in one is not evidence.

- **`reading inbox/` and the Web Clipper**, 2026-08-19. Seven weeks produced 175 clips and one source note. Capture was frictionless, so nothing was weighed before it was kept, and the full-text artifact read as progress without being any — [[Insecurity work is dangerous because it is frictionless]] names the mechanism.
- **`writing/`**, 2026-08-19. Two drafts that were neither essays nor zettels: no frontmatter, no header, no links in either direction.
- **`templates/` and `attachments/`**, 2026-08-19, with `.obsidian/` and `.ignore` alongside them. One was orphaned when the Templates plugin went, the other empty and held open by a `.gitkeep` — a folder that existed to exist.
- **Four root documents**, 2026-08-19, measured at zero links in either direction.
- **`projects/` and `sources/`**, 2026-08-20, holding three files between them.
- **The project and learning indexes**, 2026-08-20. A list of open commitments is an inventory, and the task manager already holds every item on it.
- **Checklists and next-action fields in the project notes**, 2026-08-20, as the stale second copy of the task manager's contents.
- **The review routine**, 2026-08-20, when the task system moved out of the vault.
- **The `_` filename prefix on the system notes**, 2026-08-20, once two files were all that was left to sort above the zettels.
