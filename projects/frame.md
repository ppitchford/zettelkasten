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

- **Tier 1 — v1:** 
	- Shipped region / window / full screen / scrolling capture, Quick Access Overlay (QAO), annotation editor through text, and cutover. 
	- Open threads: 
		- Self-timer (scoped, not started)
		- Dual-display verification (blocked on hardware)
		- Crop's tier (undecided, leaning Tier 2)
		- Retiring the old `screenshot` wrapper (soak period)
- **Tier 2 — Candidates:** 
	- QAO UI enhancements
	- Spotlight
	- Counter / step-mark tool for numbered tutorial call outs.
	- Combine multiple captures into one canvas.
	- Floating screenshots that a user can pin above all windows.
	- OCR on-device via Tesseract with text copied to the clipboard.
	- Extend UI to browse screenshot history. 
	- All-in-one mode that allows a single keybinding to expose all capture modes – contingent on a wider release.
	- Native `.frame` format with editable annotation layers – contingent on a wider release.
	- Migrate the screen capture from `zwlr_screencopy_v1` to `ext-image-copy-capture-v1` (`eframe` → raw `wayland-client` + `tiny-skia` rewrite)– pending on compositor support for the newer protocol.
- **Tier 3 — Parked:** 
	- Screen recording
	- Click and keystroke overlays during recording
	- Webcam overlay during recording
	- Padded backgrounds for social media ready images
	- Cloud upload and shareable links

## Learning (Rust)
Building `frame` is how you're learning Rust — the project *is* the curriculum. Reach for these only when a concept needs grounding:
- [YARR — Yet Another Rust Resource](https://yarr.fyi/) — crash course aimed at experienced engineers new to low-level.
- [The Rust Book](https://doc.rust-lang.org/book/) + [Rustlings](https://github.com/rust-lang/rustlings) — canonical reference and exercises (ownership, lifetimes).

## Notes
- Source: `~/projects/frame/` — see `ROADMAP.md`, `CLAUDE.md`, `SPIKE-FINDINGS.md`.
- This note deliberately carries no task checklist; the roadmap owns the task state.
