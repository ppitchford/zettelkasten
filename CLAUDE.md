# CLAUDE.md — Notes

A working-notes vault (plain Markdown + YAML frontmatter, git), edited in Helix with the IWE language server. This file orients Claude Code sessions running in this directory.
## What this repo is
A Zettelkasten plus a task system. Notes are atomic, densely linked, and grounded in their sources. This is an ongoing intellectual practice — developing insight is the point, not note count.
## Read first
`_Style guide.md` is the authority on conventions: dates, titles, atomicity, links, tags, entry points, source-vs-synthesis, and the task system. Read it before writing anything into the vault. Do not restate its rules here — a note that violates one of them is a note to fix.
## Layout
- **Root** — zettels, seed notes (`#seedling`), and topic notes. The filename is the title; the `id` in frontmatter is the permanent address.
- **System notes** are prefixed `_` so they sort above the zettels: `_Log.md`,
  `_Style guide.md`. This file cannot take the prefix — Claude Code loads it by exact name.
- **Folders** — there are none. `projects/` and `sources/` were removed on 2026-08-20, holding two files and one between them; `reading inbox/`, `writing/`, `templates/` and `attachments/` went on 2026-08-19. Source notes keep their `Author — Title` filename, which marks them on sight without needing a directory. Root is flat and stays flat — links are the structure, not folders. Do not propose topic folders, including a return of these.
## Where the work is
`_Log.md` — newest day on top — is the running record and capture surface, and it is
the only system note left besides the style guide. Open tasks live in Things3, not
here; a `**To route:**` block under a date is a historical artifact of the old system.
`_Projects.md` and `_Learning.md` were deleted on 2026-08-20 — the commitment ladder
and the study list were inventories, and inventories belong in Things3. Do not
recreate either one here.
## Division of labor — important
- **Claude Code does:** mechanical vault work — batch renames and `git mv`, link rewrites and broken-link verification, frontmatter and template operations, scaffolding shells for lists already specified elsewhere.
- **Claude Code does NOT:** draft zettel content from sources, or write titles. Processing a source into notes is collaborative and one-at-a-time — drafted, critiqued, revised in an interactive chat. Titles are written by hand after the idea is fully formed. Never bulk-generate note bodies.
## Git
- Upstream tracking is configured on `main`; `git push` alone suffices.
- **Verify a clean tree before starting.** This machine is now the only writer — the iOS phone and `obsidian-git` are gone — so a pull is a no-op, but a dirty tree means work in progress that is not yours to commit.
- Commit at logical checkpoints with clear messages; prefer a branch for anything that touches many files.
- `.zk/` is gitignored and dead (legacy `zk` plugin era, retired with zk-nvim). Not in version control, so deleting it is irreversible.
## Environment
- Helix (`hx`, xbps) with the IWE language server. Obsidian was removed on 2026-08-19 along with its Web Clipper; there is no GUI editor and no mobile capture surface.
- Python is available for batch operations.

## IWE
The vault is a graph indexed by `iwes`. Three things about its model are not guessable:
- A document is addressed by **key** — its path without the extension. The vault is flat, so a key is just the filename: `_Log`, not `_Log.md`.
- Links are bare wiki names (`[[Desktop made for one]]`) that resolve by shortest unambiguous path suffix. This works only while every filename in the vault is unique. Check before adding a file whose basename already exists.
- A link **alone as its own paragraph** is an inclusion link and means structural nesting — `iwe squash` pulls the target's whole body in as a nested section. A link inside a list item or inline in a sentence is a cross-reference, gloss or no gloss. Verified 2026-08-20; the distinction is the paragraph, not the line.

Three hazards:
- **Never run `iwe normalize`**, and never call the `iwe_normalize` MCP tool. It rewrites nearly every file in the vault in one command. `.helix/languages.toml` sets `auto-format = false` for the same reason — do not turn it on.
- **`iwe rename` reformats every document it updates.** Moving `projects/Desktop made for one` to root rewrote 279 lines of `_Log.md` to change one link: every `*` bullet became `-`, the frontmatter quoting changed, and blank lines were inserted throughout. Verified 2026-08-20. Use `git mv` for moves — because `wiki_link_path = "preserve"` and every link is a bare name, moving a file between directories needs no link updates at all. `iwe rename` is only safe when it reports `Updated 0 document(s)`.
- `wiki_link_path = "preserve"` in `.iwe/config.toml` is deliberate. `iwe init` detected and chose `"short"`, which rewrites link paths as documents move. Renaming a note as its wording sharpens is routine here, so it must stay `"preserve"`.