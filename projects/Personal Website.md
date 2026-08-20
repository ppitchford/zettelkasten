---
id: "20260722180200"
date: 2026-07-22
tags:
  - project
---
# Personal Website

Personal site written plain vanilla — hand-authored HTML, CSS, and JavaScript served exactly as written, with no build step, framework, or transpiler.

**Status:** planned

## Approach
[Plain vanilla web](https://plainvanillaweb.com/index.html) is the reference. The trade it names is short-term comfort for long-term simplicity and effectively zero maintenance — nothing underneath the site can rot, because there is nothing underneath it.

- **No build step.** Files ship as authored, so an edit is a deploy. Nothing to reinstall or re-resolve first.
- **Web Components** are the abstraction layer — custom elements on standard primitives, not a component framework.
- **Modern CSS** covers what preprocessors used to: nesting, custom properties, cascade layers. No SASS, no PostCSS.
- **Vanilla routing and state**, if the site ever grows past static pages.

It assumes existing fluency in the three languages rather than teaching them — this is an eject button on tooling, not a first project.

## Notes
- No source path yet. The previous attempt lived at `~/projects/website` — a Hugo scaffold, two commits, last touched 2026-06-10, never deployed and never given a remote. Deleted 2026-08-17 in favour of starting empty, since Hugo was itself the build step and theme layer this approach removes.
- Plausible analytics is the one task carried over from that attempt.
