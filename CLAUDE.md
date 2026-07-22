# CLAUDE.md — Zettelkasten

A working-notes vault (Obsidian, plain Markdown + YAML frontmatter, git). This file
orients Claude Code sessions running in this directory.

## What this repo is
A Zettelkasten plus a task system. Notes are atomic, densely linked, and grounded in
their sources. This is an ongoing intellectual practice — developing insight is the
point, not note count.

## Active work
See `TODO.md` in the root: a six-phase restructure and Things3 migration. Work the
phases in dependency order. **Phase 1 (conventions & structure) underpins everything
and must be completed first** — the flatten and link-rewrite are prerequisites for the
rest.

## Conventions (do not deviate)
- **Frontmatter:** `id` (timestamp `YYYYMMDDHHmmss`), `date` (`YYYY-MM-DD`); `aliases`
  for human-readable backlinks. Titles are written manually, after the idea is fully
  formed — never auto-generate them.
- **Dates:** `YYYY-MM-DD` everywhere.
- **Links:** `—` (em dash) as the separator. Link descriptions are directional and
  relational — never circular or categorical. Leave a link empty rather than forcing a
  connection that isn't honest.
- **Titles:** state the atomic claim directly; hedge deliberately (`may` vs.
  deterministic framing); no semicolons.
- **Atomicity:** one idea per note. Bodies carry the mechanism without restating the
  title; each sentence carries distinct weight.
- **Source vs. synthesis:** a note body stays grounded in its single source. Original
  synthesis and cross-source connections belong in *separate linked notes* — never
  imported into a source note's body.

## Division of labor — important
- **Claude Code does:** the mechanical restructure — move own-notes to root, rewrite
  path-qualified wikilinks to bare links, convert template placeholders to Obsidian
  syntax, scaffold note shells (project notes, `Projects.md`, the reading-inbox note,
  `Someday.md`), and populate the *lists already specified* in the TODO.md appendices.
- **Claude Code does NOT:** draft zettel intellectual content from source articles.
  Processing reading-inbox items into zettels is collaborative and one-at-a-time —
  drafted, critiqued, and revised in an interactive chat. Do not bulk-generate note
  bodies from sources.

## Git
- Upstream tracking is configured on `main`; `git push` alone suffices.
- Commit at logical checkpoints with clear messages. Prefer a branch for the
  flatten + link-rewrite.
- Before the flatten: verify a clean working tree. After: verify no links broke.
- `.zk/` is gitignored (legacy `zk` plugin era).

## Environment
- Obsidian (AppImage on Void Linux / Wayland, `--ozone-platform=wayland`). Core
  Templates plugin handles `id`/`date` scaffolding.
- Python is available for batch operations (link rewrite, alias injection).
