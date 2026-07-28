---
id: "20260727120000"
date: 2026-07-27
---
# Learning plan

How skill-building attaches to shipping. **One track active at a time; the rest stays parked.** Courses are lossy — extract what a project needs and move on. A learning line you're *actively doing* is a project, not a someday item. The apps are the curriculum.

## Plan A — Ship web apps (active)
Backend in **Python and/or Go**; Node is optional JS literacy, not on the path. Build, and pull courses in as they block you.

1. **[Complete Intro to SQL & PostgreSQL](https://frontendmasters.com/courses/sql/)** (7h20m) — the one course here that's stack-neutral: joins, indexes, JSONB, transactions, window functions. Both lanes sit on Postgres. Do it early.
2. **[[Personal Website]] — ship first, minimal.** Little/no backend; the real skill is deployment (DNS, HTTPS, CI/CD). Get a static/light site live on a host. Momentum over sophistication.
3. **[[ArtSpacesSoCal]] — the real backend. Pick a lane:**
   - **Python (fastest to ship):** [FastAPI tutorial](https://fastapi.tiangolo.com/tutorial/) — typed, auto-generated OpenAPI docs; the best modern API-design teacher. (Frontend Masters has no Python track.)
   - **Go (for Go depth):** [Go & Vanilla JS: Fullstack Without Frameworks](https://frontendmasters.com/courses/vanilla-js-go/) (10h11m) — a full app in Go's stdlib + Postgres + JWT + a vanilla-JS frontend. Prereq is *Basics of Go* → do [A Tour of Go](https://go.dev/tour/) then [Learn Go with Tests](https://quii.gitbook.io/learn-go-with-tests/) first. Day-to-day reference: [gosamples.dev](https://gosamples.dev/).
4. **Deploy — just-in-time:** *Complete Intro to Containers* (Docker) + the deploy half of *Full Stack Fundamentals* only when a real deploy needs them.

## Rust — systems & tools (via `frame`)
You're learning Rust by building **[[frame]]** with Claude — the right way to do it. Supplement:
- [YARR — Yet Another Rust Resource](https://yarr.fyi/) — a Rust crash course aimed at experienced engineers new to low-level. Your best-fit starting point.
- [The Rust Book](https://doc.rust-lang.org/book/) + [Rustlings](https://github.com/rust-lang/rustlings) — canonical reference and exercises for when a concept (ownership, lifetimes) needs grounding.

## Plan B — CS fundamentals (secondary: credibility + interviews; light, ongoing)
- **grokking Algorithms, 2e** — primary. Gentle, illustrated, and its examples are in **Python** — matches your stack. Enough breadth for a technical PM.
- **[The Missing Semester](https://missing.csail.mit.edu/)** — a weekend tune-up. You have most, but debugging/profiling, packaging, code-quality, and the new-for-2026 **Agentic Coding** lecture are worth a skim.
- **Designing Data-Intensive Applications** (Kleppmann) — *added.* The single most career-valuable systems book for a technical PM; far more relevant than CLRS.
- Deferred: the Frontend Masters algorithms videos + CLRS — interview prep / shelf reference only.

## Plan C — Math / ML / creative (parked; for joy, long horizon)
- **[The Little Book of Linear Algebra](https://little-book-of.github.io/linear-algebra/)** — foundation; before any ML.
- **[The Little Learner](https://www.thelittlelearner.com/)** — deep learning from first principles, in **Racket** (a detour from your stack: excellent for *understanding* NN internals, wrong tool for *practical* Python ML — use PyTorch/fast.ai for that). Feeds GuppyLM if it ever activates.
- **[The Nature of Code](https://natureofcode.com/)** — creative simulation in JS/p5.js (forces, flocking, fractals, genetic algorithms). Pure play.

## Pruned / deferred
- **Skip:** *Complete Intro to Linux & CLI*, *VIM Fundamentals* (you live in both); *Learn Go the Hard Way* / *Learn Python the Hard Way* (replaced by Tour of Go + Learn Go with Tests; Python via FastAPI + doing).
- **Redundant:** *Complete Intro to Databases* (five-database survey) — Postgres via the SQL course is enough.
- **Defer:** *Backend System Design*, *Advanced Algorithms Part 2*, *CLRS*.
- **Optional literacy only:** *Intro to Node.js* + *API Design in Node* — for JS fluency, off the critical path.

## Tooling / enrichment (not on the critical path)
- [Learn Neovim](https://ofirgall.github.io/learn-nvim/) — sharpen the editor you already use; pick up when you want to level up motions/config.
- [Linux Basics for Hackers — notes](https://github.com/ahegazy0/linux-basics-for-hackers-notes) — Linux + security enrichment; parked-interest, not job-critical.
