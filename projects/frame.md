---
id: "20260722180100"
date: 2026-07-22
aliases:
  - frame
tags:
  - project
---
# frame

Screenshot tool for Void / MangoWM (Wayland), written in Rust; replaces the `~/.local/bin/screenshot` wrapper.

**Status:** v1 shipped — Tier 1 complete, cutover landed 2026-07-21
**Next action:** Quick Access Overlay Rosé Pine theming (first Tier 2 move)

## Roadmap
The living, authoritative roadmap is `~/projects/frame/ROADMAP.md`. This note is a pointer and status summary, not a copy — track detail there so the two never drift.

- **Tier 1 — v1:** shipped (region / window / fullscreen / scrolling capture, Quick Access Overlay, annotation editor through text, cutover). Open threads: self-timer (scoped, not started), dual-display verification (blocked on hardware), crop's tier (undecided, leaning Tier 2), retiring the old `screenshot` wrapper (soak period).
- **Tier 2 — follow-on (gate open):** first move is QAO Rosé Pine theming. Candidates include spotlight, counter / step-mark, freeze-screen mode, multi-capture canvas, window-composite backgrounds, floating screenshots, OCR, and the `eframe` → raw `wayland-client` + `tiny-skia` rewrite.
- **Tier 3 — parked:** screen recording, recording overlays, webcam overlay, padded social backgrounds, cloud upload.

## Notes
- Source: `~/projects/frame/` — see `ROADMAP.md`, `CLAUDE.md`, `SPIKE-FINDINGS.md`.
- This note deliberately carries no task checklist; the roadmap owns the task state.
