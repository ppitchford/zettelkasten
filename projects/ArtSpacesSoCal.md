---
id: "20260722180300"
date: 2026-07-22
tags:
  - project
---
# ArtSpacesSoCal

A boutique search engine for Southern California art galleries and events — and the **learn-by-building** vehicle for shipping a Go web app. Source: `~/projects/artspacesocal/`.

**Status:** active
**Next action:** work through [A Tour of Go](https://go.dev/tour/) as a refresher, then walk `main.go` / `db.go` / `auth.go` to understand the foundation you own.

## Stack (decided — already built)
Go stdlib `net/http` (no framework), `modernc.org/sqlite` (pure-Go, static binary), `smhanov/auth` for sessions. Migrations embedded + numbered. Deployed to Hetzner via `deploy.sh` + systemd; DB backed up. The infra is done — the *app* is the greenfield.

## Shipped = (lean v1)
Deployed on Hetzner, a visitor can **search SoCal galleries by name/city and open a gallery detail page**, on real seed data. That's the finish line — it delivers the core promise and keeps this from becoming learn-forever.

## Roadmap
1. **Refresh Go** — [A Tour of Go](https://go.dev/tour/): re-ground syntax and idioms (structs, methods, slices/maps, errors, interfaces) so building is productive struggle, not floundering.
2. **Own the foundation** — read the existing `main.go`, `db.go`, `auth.go`, `deploy.sh`; understand every line before adding to it.
3. **Galleries browse** — a handler + `html/template` for a gallery list and a detail page. *First moment the app is real.* → Go handlers, templating, querying SQLite.
4. **Search (FTS5)** — an FTS5 virtual table over galleries, a `/search` handler + results page. → SQLite full-text search. *This is the "search engine."*
5. **Ship v1** — seed real SoCal galleries, deploy, verify live.
- Fast-follows (v1.1+): `events` (migration `002`, belongs-to gallery), `artists`, a map view (lat/lon already stored), saved galleries (auth is already there).

## How we work (learning agreement)
A **learning** project: Claude is tutor/reviewer, **not** the implementer. **You write every line of application code.**
- **Start explain-then-write**, and **fade deliberately**: once a pattern is familiar, drop to *review-after* (attempt solo, then I critique), then *hint-only* for the routine. Tell: if my explanation makes you think "I could've written that," attempt-first next time.
- **Guardrail:** I explain the concept and point at the docs/pattern — I do *not* write the code in my explanation, or "you write it" collapses into transcription.

## Learning references (pull just-in-time)
- **Primary:** your own foundation code (same exact stack) + the Go stdlib docs (`net/http`, `html/template`, `database/sql`).
- **Patterns:** [Go & Vanilla JS: Fullstack Without Frameworks](https://frontendmasters.com/courses/vanilla-js-go/) — the relevant section per milestone, not a binge. It uses Postgres + JWT (you use SQLite + sessions), so you *translate* the pattern rather than copy it.
- **Go refresher:** [A Tour of Go](https://go.dev/tour/) — the roadmap's first step; re-ground syntax before building.

## Notes
> Context, source paths, and honest links to related notes.
