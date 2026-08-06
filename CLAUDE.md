# CLAUDE.md — Zettelkasten

A working-notes vault (Obsidian, plain Markdown + YAML frontmatter, git). This file
orients Claude Code sessions running in this directory.
## What this repo is
A Zettelkasten plus a task system. Notes are atomic, densely linked, and grounded in
their sources. This is an ongoing intellectual practice — developing insight is the
point, not note count.
## Read first
`_Style guide.md` is the authority on conventions: dates, titles, atomicity, links,
tags, entry points, source-vs-synthesis, and the task system. Read it before writing
anything into the vault. Do not restate its rules here — a note that violates one of
them is a note to fix.
## Layout
- **Root** — zettels, seed notes (`#seedling`), and topic notes. The filename is the
  title; the `id` in frontmatter is the permanent address.
- **System notes** are prefixed `_` so they sort above the zettels: `_Log.md`,
  `_Style guide.md`, `_Learning.md`, `projects/_Projects.md`. This file cannot take the
  prefix — Claude Code loads it by exact name.
- **Folders** — `projects/`, `reading inbox/` (clipped articles awaiting judgment),
  `writing/` (original long-form), `templates/`, `attachments/`.
## Where the work is
`_Log.md` — newest day on top — is the running record and capture surface; open tasks
sit under the current date in a `**To route:**` block. `projects/_Projects.md` indexes
active, planned, and someday projects. There is no separate task file.
## Division of labor — important
- **Claude Code does:** mechanical vault work — batch renames and `git mv`, link
  rewrites and broken-link verification, frontmatter and template operations, scaffolding
  shells for lists already specified elsewhere.
- **Claude Code does NOT:** draft zettel content from sources, or write titles.
  Processing a source into notes is collaborative and one-at-a-time — drafted,
  critiqued, revised in an interactive chat. Titles are written by hand after the idea
  is fully formed. Never bulk-generate note bodies.
## Git
- Upstream tracking is configured on `main`; `git push` alone suffices.
- **Pull before editing.** The vault syncs to an iOS phone via `obsidian-git`, so
  `_Log.md` may be ahead of local. Verify a clean tree before starting.
- Commit at logical checkpoints with clear messages; prefer a branch for anything that
  touches many files.
- `.zk/` is gitignored (legacy `zk` plugin era).
## Environment
- Obsidian (AppImage on Void Linux / Wayland, `--ozone-platform=wayland`). Core
  Templates plugin scaffolds `id`/`date` from `templates/note`.
- Python is available for batch operations.