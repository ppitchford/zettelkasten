---
title: "Beyond the Prompt: Claude Code"
source: "https://www.instapaper.com/read/2025010268"
author:
published: 19 min
created: 2026-07-24
---
[arps18.github.io](https://arps18.github.io/posts/claude-code-mastery/)

I burned forty minutes on a refactor `claude` could’ve shipped in four, and the gap wasn’t the model. It was everything around it. The casual user types prompts, takes the first suggestion, and treats `claude` like dressed-up autocomplete. I run it as a programmable agent: persistent memory, custom commands, parallel sessions spawning across worktrees, and a project layout that gets sharper every week I touch it. This guide assumes you’ve already typed `claude` into a terminal and seen what happens. We’re going past that…

Now the fun part.

## 1\. Claude Code Beyond the Basics

Stop treating Claude Code like a prompt-and-wait chatbot. Treat it like an autonomous agent that needs guardrails, and your whole workflow shifts. The core principle from Boris Cherny and the Anthropic team is simple, and it’s to **give Claude a way to verify its own work**. Without that loop, you’re the only feedback signal. With it, Claude iterates until the code actually runs. Boris pegs this single move at a 2-3x quality bump.

A few patterns that shift how you operate day to day.

**Explore, then plan, then code.** Hit `Shift+Tab` twice to drop into plan mode, which is read-only. Let Claude read files, trace flows, map the data model. Get a plan back. Then execute. Skip planning for small fixes. Use it the moment a change touches more than one file.

**Treat plan mode like a design doc.** Have one Claude write the plan. Spin up a second Claude in a fresh session and ask it to review the plan as a staff engineer, no context bias, so it actually catches gaps. If the implementation goes sideways, go back to plan mode and re-plan with verification steps baked in.

**Reference, don’t describe.** Skip “look at the auth module” and type `@src/auth/login.py`. Skip pasting an error and pipe it instead with `cat error.log | claude`. Exact context beats approximate description every single time.

**Delegate, don’t pair-program.** Cat Wu (Claude Code team) puts it plainly: “The model performs best if you treat it like an engineer you’re delegating to, not a pair programmer you’re guiding line by line.” Write a crisp brief upfront. Then let it run.

> **:** Press `Ctrl+G` to open Claude’s plan in your editor and tweak it before Claude proceeds. The plan is just text, so shape it before it becomes code.

> **:** When Claude makes a mistake, end your prompt with “Update CLAUDE.md so you don’t repeat this.” Boris calls Claude “eerily good at writing rules for itself” from its own failures. Do note, this one habit compounds harder than anything else in this guide.

## 2\. The.claude Directory, Properly Understood

Most folks crack open `.claude/`, spot `CLAUDE.md`, and bounce. It’s a layered config system underneath.

Two scopes. Project scope sits in `.claude/` inside the repo, committed so the team shares it. Global scope sits in `~/.claude/` and rides along on every project on your box.

Mental model. Project files describe the project, global files describe you.

FileScopeCommitWhat it does `CLAUDE.md` Project and globalYesInstructions loaded every session `CLAUDE.local.md` Project onlyNo, gitignore itYour private project notes `settings.json` Project and globalYesPermissions, hooks, env vars, model defaults `settings.local.json` Project onlyNoPersonal overrides, auto-gitignored`.mcp.json` Project onlyYesTeam-shared MCP servers `skills/<name>/SKILL.md` Project and globalYesReusable prompts invoked with `/name` `commands/*.md` Project and globalYesSingle-file slash commands `agents/*.md` Project and globalYesSubagent definitions `rules/*.md` Project and globalYesTopic-scoped instructions, optionally path-gated

A typical layout.

```js
1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
```
```js
my-repo/
├── .claude/
│   ├── settings.json
│   ├── agents/
│   │   ├── pr-review.md
│   │   └── test-writer.md
│   ├── skills/
│   │   └── api-conventions/SKILL.md
│   └── rules/
│       ├── frontend.md        # path-gated to src/frontend/
│       └── migrations.md      # path-gated to db/migrations/
├── CLAUDE.md                  # checked in, team-shared
├── CLAUDE.local.md            # gitignored, personal
└── .mcp.json                  # team-shared MCP servers
```

A few easy misses.

`CLAUDE.md` **files cascade.** In a monorepo, both `root/CLAUDE.md` and `root/services/billing/CLAUDE.md` load while you’re working the billing service. Handy when folder conventions diverge.

`rules/*.md` **is path-gated.** Guidance specific to your migrations folder shouldn’t bloat every session through `CLAUDE.md`, rather it belongs in `.claude/rules/migrations.md` with a glob.

**Skills beat commands.** Both `.claude/commands/*.md` and `.claude/skills/<name>/SKILL.md` register slash commands, but skills carry supporting files, `disable-model-invocation`, allowed tools, and agent overrides. I shipped a chunk of repo automation as loose `commands/*.md` last year and had to port the whole lot to `skills/` once supporting files were needed. New work goes in `skills/`.

> **:** Run `claude project purge ~/path/to/repo --dry-run` to see exactly what local state Claude holds for a project, handy before handing off a laptop.

## 3\. CLAUDE.md, The Way Boris Writes It

`CLAUDE.md` loads at the start of every session. Get it wrong, Claude keeps tripping on the same rake. Get it right, the same prompt suddenly produces output you’d actually ship.

Boris cares about two things here, and the rest is noise once you’ve got these down. I spent a month writing increasingly elaborate context files before I came back around to his framing, and the elaborate versions were worse in every measurable way…

**Keep it short.** Long files bury the rules that actually matter. For every line you write, run it through Boris’s filter and ask, “Would removing this cause Claude to make a mistake?” If the answer is no, cut it. Be ruthless. The file isn’t a knowledge base, it’s a guardrail.

**Let Claude write rules for itself.** Any time Claude does something wrong, tell it to “Update CLAUDE.md so you don’t repeat this.” Claude is surprisingly good at distilling its own mistakes into precise rules. Do this for a couple of weeks and the file turns into a curated list of every gotcha your project has accumulated, written in the exact phrasing the model responds to. You stop guessing what to put in there because the model tells you.

### 3.1 The Real CLAUDE.md From the Claude Code Team

In one of his talks, Boris walked through the actual `CLAUDE.md` the Claude Code team checks into their own repo. The whole team contributes to it multiple times a week.

```js
1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
```
```js
# Development Workflow

**Always use \`bun\`, not \`npm\`.**

# 1. Make changes

# 2. Typecheck (fast)

bun run typecheck

# 3. Run tests

bun run test -- -t "test name" # Single suite
bun run test:file -- "glob" # Specific files

# 4. Lint before committing

bun run lint:file -- "file1.ts"
bun run lint

# 5. Before creating PR

bun run lint:claude && bun run test
```

Read it again. Build commands Claude can’t guess, the exact order to run things, single-test invocations, the pre-PR ritual. No style preferences, no codebase tours, no platitudes.

Boris also uses `@claude` in PR comments to have Claude commit a rule directly:

```js
1
2
```
```js
nit: use a string literal, not a ts enum
@claude add to CLAUDE.md to never use enums, always prefer literal unions
```

He calls this “Compounding Engineering,” where every PR review becomes a `CLAUDE.md` improvement. The reviewer catches a mistake once, and Claude never repeats it.

Here’s a fleshed-out template following the same philosophy:

```js
1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
```
```js
# Code style

- Use ES modules (import/export), not CommonJS (require)

# Workflow

- Always use \`bun\`, not \`npm\`
- Run \`bun run typecheck\` before claiming done
- Never push to main directly. Always open a PR.

# Architecture

- All API routes go through src/api/middleware/auth.ts
- New database queries go in src/db/queries/. No inline raw SQL.

# Gotchas

- \`User\` and \`UserRecord\` are distinct types. UserRecord is the DB row, User is the runtime object.
- \`formatCurrency\` assumes USD. For international use \`formatCurrencyByLocale\`.
```

Pay attention to the “Gotchas” section. Every entry there started as a real mistake Claude made on a real PR, captured the moment it happened. That sinking moment when you realize the model just shipped USD formatting to a French user? You write it down once, and it never happens again.

**Skip these in `CLAUDE.md`:** standard language conventions, file-by-file codebase descriptions, long tutorials, API docs, anything that changes frequently.

> **:** Words like `IMPORTANT` or `YOU MUST` improve adherence. Use them sparingly so they carry weight.

You can import other files using `@path` syntax to keep `CLAUDE.md` short while pulling in details on demand:

```js
1
2
```
```js
See @README.md for project overview and @package.json for scripts.
@~/.claude/my-preferences.md
```

Short file, huge payoff. Keep it tight.

### 3.2 Popular CLAUDE.md Files Worth Studying

Steal from people who’ve already done the work. These four are the ones I keep coming back to.

- **[mattpocock/skills CLAUDE.md](https://github.com/mattpocock/skills/blob/main/CLAUDE.md)** : conventions for how skills should be written and tested
- **[anthropics/claude-code-action](https://github.com/anthropics/claude-code-action)** : Anthropic’s own repo, treated the same as internal tools
- **[awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code)** : links to dozens of public `CLAUDE.md` files across language ecosystems
- **[claudelog.com](https://claudelog.com/)** : community-curated examples organized by stack

Read three, then write yours.

## 4\. CLAUDE.local.md as a Daily Driver

`CLAUDE.local.md` sits next to `CLAUDE.md`, loads the same way, and never leaves my machine. It goes straight into `.gitignore`.

Here’s how I run it. After every PR, reviewers drop comments. Instead of trying to hold them in my head, I paste them into `CLAUDE.local.md` the second I read them. Over a few weeks it turns into a personal rule file tuned to the exact feedback I keep getting.

```js
1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
```
```js
# Personal review notes (private)

# From PR feedback

- New SQS consumers need a DLQ and alarms in the same PR
- Use \`Optional<T>\` over null returns
- Tests for new endpoints must include the auth-failure case
- Prefer named tuples over plain dicts for return types with 3+ fields

# My own quirks to correct

- Stop using \`console.log\`; use the project logger instead
- Always update the OpenAPI spec when adding endpoints
```

Loaded every session. Claude now includes the auth-failure test and updates the OpenAPI spec without me asking. Nitpick comments on my PRs dropped within two weeks.

> **:** Keep two sections clearly separated: project-specific feedback and personal habits to correct. Mixing them makes the file harder to prune later.

> **:** Prune after a few weeks. Anything that’s become muscle memory can go. The file should capture what’s still in flux; the stuff that runs on autopilot can go.

## 5\. Skills, In Depth

Skills are how you take Claude Code from “agent that can do anything” to “agent that does the three specific things your project actually needs, done the way your team does them.” They’re the unit of reusable expertise you’ll keep reaching for once you’ve written one or two…

### 5.1 What Skills Actually Are

Last Tuesday I needed Claude to summarize my uncommitted diff the same way every time, so I dropped a folder into `~/.claude/skills/` and walked away. That folder is the skill. Inside lives a `SKILL.md` carrying frontmatter and instructions, and the folder name itself becomes the slash command you type at the prompt. Project-scoped ones sit under `.claude/skills/<name>/`, global ones under `~/.claude/skills/<name>/`.

Here’s the smallest version that earns its keep:

```js
1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
```
```js
---
description: Summarizes uncommitted changes and flags anything risky. Use when the user asks what changed, wants a commit message, or asks to review their diff.
---

## Current changes

!\`git diff HEAD\`

## Instructions

Summarize the changes in two or three bullet points, then list any risks: missing error handling, hardcoded values, tests that need updating.
```

Save that to `~/.claude/skills/summarize-changes/SKILL.md` and `/summarize-changes` shows up in every session you open after.

**Three things that make Skills powerful:**

- **Progressive disclosure.** At session start Claude only reads the frontmatter descriptions, roughly 100 tokens apiece. The full `SKILL.md` and any helper files don’t get pulled in until the skill actually fires.
- **Each skill lives as its own folder**, so you can drop a `templates/` directory alongside the `SKILL.md`, stash reference docs next to it, and keep scripts in the same tree. The `SKILL.md` is just the entry point you hand to Claude.
- **Inline shell.** Any line starting with `!` runs the command at invocation time and splices the output straight into the prompt.

The frontmatter itself carries a good amount of optional knobs:

```js
1
2
3
4
5
6
7
```
```js
---
name: my-skill
description: When to use this skill
disable-model-invocation: true # only runs when user explicitly types /my-skill
allowed-tools: Read, Grep, Bash
agent: read-only
---
```

> **:** Use `disable-model-invocation: true` for skills with side effects. You want `/ship` to deploy only when explicitly typed, not when Claude decides it’s relevant.

Set it once. Forget it forever.

### 5.2 Writing a Real Skill: Go API Conventions

Here’s a complete skill for a Go service team. It carries the conventions, the gotchas, and scaffolding for a fresh HTTP handler.

```js
1
2
3
4
5
6
```
```js
.claude/skills/go-handler/
├── SKILL.md
├── templates/
│   └── handler.go.tmpl
└── examples/
    └── healthz.go
```

```js
1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
```
```js
---
description: Scaffolds a new HTTP handler in our Go service following team conventions for routing, validation, error handling, and tests. Use when the user asks to add a new endpoint, a new handler, or extend an existing route group.
---

# Go HTTP Handler Skill

## Stack

- Go 1.22 with chi router
- sqlc for typed queries, never write raw SQL strings in handlers
- zap for structured logging, never fmt.Println
- testify for assertions, table-driven tests preferred

## Gotchas

- \`chi.URLParam\` returns \`""\` for missing params, not an error. Always check.
- Our \`httperr.Wrap\` doesn't log. Log separately with \`h.log.Error\` before returning.
- Auth middleware injects via \`context.Value(authkey.User)\`. Type-assert to \`*models.User\`.
- sqlc nullable strings use \`pgtype.Text\`. Check \`.Valid\` before calling \`.String\`.
- Tests must use \`httptest.NewRecorder\` and \`httptest.NewRequest\`. No real server.
```

Do note what’s happening here. A new developer can ship a fully conventional endpoint on day one without spelunking through the codebase first.

### 5.3 Popular Skills Worth Installing

**[mattpocock/skills](https://github.com/mattpocock/skills)** is the most popular skills repo, sitting around 100k stars. A few I keep loaded:

- `/grill-me`: interviews you about a plan before any code gets written
- `/tdd`: enforces red-green-refactor strictly
- `/diagnose`: disciplined debugging, reproduce, minimize, hypothesize, fix, regression test

Install with `npx skills@latest add mattpocock/skills`.

**[Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills)** ships 66 language-specific profiles like `go-pro`, `python-pro`, `java-architect`, `typescript-pro`, `rust-engineer`, `sql-pro`, and more. You compose them. A Next.js task pulls in `nextjs-developer` alongside `typescript-pro`.

**Anthropic’s official skills:**

- `/code-review`: four parallel agents audit the diff, confidence-scored findings only
- `/simplify`: reviews recent code for reuse and efficiency
- `/batch`: fans out a migration to dozens of parallel agents, every one isolated in a worktree
- `/webapp-testing`: gives Claude Playwright control to test your local web app

I used to write the same prompt three times a week before it clicked…

> **:** If you do something more than once a day, turn it into a skill. Anything you repeat is a skill waiting to be written.

> **:** Check skills into git. They become institutional knowledge, and new engineers clone the repo and get the team’s accumulated practices for free.

## 6\. Building Custom Subagents

Spin one up and it’ll chew through fifty files without bloating your main session, then hand back a tidy summary. Isolated context. Scoped tool permissions. Separate blast radius. I started reaching for them after watching a single debugging session torch my context budget chasing imports across a monorepo, and now I default to them.

Drop a markdown file into `.claude/agents/` for project scope, or `~/.claude/agents/` if you want it globally available. The frontmatter declares name, description, tools, and model. Five lines, full contract.

### 6.1 Walking Through a /pr-review Agent

Last Friday I almost shipped a PR with a missing null check, and that’s when I built this agent.

```js
1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
```
```js
---
name: pr-review
description: Reviews the current branch diff against main, looking for bugs, security issues, missed edge cases, and project-convention violations. Use proactively before opening a PR.
tools: Read, Grep, Glob, Bash
model: opus
---

You are a senior staff engineer reviewing a pull request. Thorough, direct, goal is to catch issues before human reviewers do.

## Process

1. Run \`git diff main...HEAD\`
2. Run \`git log main..HEAD --oneline\`
3. Read full files, not just diff context
4. Cross-check against CLAUDE.md, CLAUDE.local.md, and .claude/rules/

## Flag

- Correctness bugs: off-by-one, null handling, error paths, race conditions
- Security: injection risks, missing auth checks, secrets in code
- Missing tests for new logic
- N+1 queries
- Convention violations from CLAUDE.md or rules/

## Do NOT flag

- Style preferences not in project rules
- Refactoring suggestions for working code
- Anything outside this diff

## Output

Group by severity (Critical / High / Medium / Low). File + line + issue + suggested fix.
End with a verdict: **SHIP**, **FIX FIRST**, or **REWORK**.
```

I trigger it by typing `Have the pr-review agent look at my current branch.` into the session. The subagent does its work in an isolated context, and my main session doesn’t get cluttered with review chatter.

A few choices worth calling out. The `tools` list is deliberately read-only, since a reviewer that can patch code starts rationalizing its own fixes instead of flagging them. I picked `model: opus` because catching a security bug before a human reviewer does is worth the cost. By the way, the “Do NOT flag” section is what actually makes the output usable. Without it I’d drown in nitpicks about variable naming.

Built it in ten minutes. Saved me twice already.

### 6.2 Popular Subagents Worth Stealing

Straight from the Claude Code team’s own workflow, you’ll find `build-validator`, `code-architect`, `code-simplifier`, `oncall-guide`, and `verify-app` running daily.

Here’s what the community keeps reaching for…

AgentWhat it does `security-reviewer` injection, auth, secrets, insecure deserialization `test-writer` generates tests, pairs with code-reviewer in a loop `debugger` traces failing tests to root causes `performance-auditor` profiles flows and queries `migration-writer` generates DB migrations matching project conventions `release-notes-writer` changelogs from commit history

If you want curated collections, [VoltAgent/awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) ships with over 100 agents, and [hesreallyhim/a-list-of-claude-code-agents](https://github.com/hesreallyhim/a-list-of-claude-code-agents) curates another solid set.

> **:** Chain agents: Session A implements, then call `Use the code-reviewer subagent to check the work.` The reviewer evaluates in a fresh context with no implementation bias.

> **:** Add `isolation: worktree` to frontmatter to run the subagent in its own git worktree, especially powerful when fanning out a migration across dozens of parallel agents.

Steal the patterns today, and tomorrow you’ll wonder how you ever shipped without them.

## 7\. Plugins and the Marketplace

Plugins bundle skills, hooks, subagents, and MCP servers into one installable unit. Run `/plugin` to open the marketplace browser. Add community marketplaces with `/plugin marketplace add owner/repo`.

**Day-one installs:**

**`/code-review`** fires four agents in parallel. Two check `CLAUDE.md` compliance. One hunts bugs. One reads git blame for context. Output is confidence-scored. Signal stays high, noise stays low.

**`/feature-dev`** is the most-installed skill on the official marketplace. Hand it a feature brief, get working code. Seven phases. Requirements, exploration, architecture, implementation, testing, review, docs.

**Language server plugin** wires symbol-level navigation and on-edit diagnostics straight into your session. The team consistently flags it as the highest-leverage plugin you can install.

**`/security-guidance`** is Anthropic’s official security skill. Surfaces concerns before they ship.

Plugin categories worth knowing (1,000+ plugins across 75+ marketplaces as of mid-2026):

- Git workflow, code intelligence (LSP), documentation generators, testing, browser automation (Playwright), design system (Figma), observability (Sentry, Datadog)

> **:** A team-shared `.mcp.json` together with a few well-chosen plugins gets a new engineer productive within minutes of cloning the repo. Treat plugin choices as part of your onboarding story.

## 8\. Underused Claude Code Commands

Most folks learn `/clear`, `/compact`, and `/init`, then stop exploring. The rest of the command surface is where the real productivity hides, and barely anyone touches it.

CommandWhat it does `/insights` Analyzes your usage patterns; run once a month `/compact <hint>` Compresses session; hint controls what survives `/copy` Copies last response; interactive picker for code blocks `/rewind` Undo for your whole session, restoring code, conversation, or both `/btw` Side question that never enters conversation history `/context` Visualizes context usage `/export <file>` Dumps conversation to file `/branch` Forks your session to try something risky `/batch` Fans work out to parallel agents across worktrees `/loop <interval>` Schedules Claude to run on repeat, up to 3 days `/schedule` Cloud version of `/loop`, works even when your laptop is closed `/teleport` Moves a session between terminal and web `/focus` Hides intermediate tool calls, shows only final result `/voice` Voice input; Boris says he codes mostly by speaking `--bare` Up to 10x faster startup for non-interactive `claude -p` usage

Let’s dig deeper into the two that I reach for daily…

**`/compact` vs `/clear`:** if you’re starting a genuinely new task, hit `/clear` and write a fresh brief by hand. If the next task still leans on what you just did, run `/compact` with a hint about what should survive. `/compact` is a lossy LLM summary of the session. `/clear` is your own brief, written deliberately. I confused these for weeks and wondered why my sessions kept drifting. Once that distinction clicked, my context stayed cleaner for hours at a stretch.

**`/rewind`** drops a checkpoint at every prompt, and those checkpoints stick around across sessions. So when Claude wanders down a wrong path, resist the urge to type “that didn’t work, try X.” Typing that just buries the bad attempt inside your context and the model keeps tripping over it. Rewind to before the mistake, then re-prompt with whatever you learned from watching it fail. Your context window will thank you.

> **:** Use `!` as a shell escape. `!git status` or `!npm test` runs immediately with output landing in context.

> **:** Set `CLAUDE_CODE_AUTO_COMPACT_WINDOW=400000`. Context rot kicks in around 300-400k tokens on the 1M model, so force earlier compaction to stay sharp.

**Fan-out pattern:** write the task list first, then loop over it. I migrated about two thousand component files this way last quarter. Generate the list, sanity-check three of them by hand, tighten the prompt until those three come back clean, then unleash it on the rest while you go get coffee.

```js
1
2
3
4
5
```
```js
for file in $(cat files.txt); do
  claude -p "Migrate $file from React to Vue. Return OK or FAIL." \
    --allowedTools "Edit,Bash(git commit *)" \
    --bare
done
```

### 8.1 /goal, the Ralph Loop Built In

Last Tuesday I typed one line, closed the laptop, ate dinner, came back to a green PR. `/goal` sets a completion condition, and Claude keeps grinding until that condition holds true. Every attempt to stop triggers a check against the transcript.

```js
1
```
```js
/goal all tests in test/auth pass and the lint step is clean
```

Real examples:

```js
1
2
3
4
```
```js
/goal all integration tests in tests/api pass without flaking 3 runs in a row
/goal the OpenAPI spec validates and matches the actual response shapes
/goal docker compose up runs cleanly and the healthcheck endpoint returns 200
/goal coverage on src/billing/ is above 80% and all new tests are not placeholders
```

Pick something verifiable and deterministic. Tie it to a test command, a CLI exit code, or some file state you can grep for. Write “the code is good” and you’ve already lost…

Companions that pair well:

- `/loop`: repeat at an interval, burn down a backlog
- `/schedule`: run on a cadence in the cloud
- A `Stop` hook: gate on your own test suite or CI endpoint
- **Auto mode**: removes permission prompts so long goals don’t stall

> **:** Combine `/goal` + auto mode + `/focus`. Write a crisp brief, set the goal, walk away. Come back to a finished PR. This is the workflow Boris and Cat Wu push for Opus 4.7.

## 9\. MCPs as Power Tools

MCP (Model Context Protocol) is the wire that turns Claude Code from a coding agent into a system-aware one. An MCP server exposes external tools (a database, a design canvas, your error tracker, your notes) to Claude through a standard contract, so the agent can call them like any other tool.

Without MCP, Claude reads files and runs commands. With MCP, it reads your `Linear` tickets, queries your `Postgres`, pulls up a `Figma` component, fetches live `Sentry` stack traces, or reads your `Obsidian` vault, all without you leaving the terminal.

**The go-to MCPs for engineering work:**

MCPWhat it unlocksGitHubRepo management, PRs, issues, code searchContext7Live, up-to-date library docs; append `use context7` to any promptSentryReal error context, stack traces, breadcrumbsLinearRead/create tickets, update statusPlaywrightBrowser automation via accessibility snapshotsFigmaLive design tree: auto-layout, spacing tokens, component refsPostgres / SupabaseQuery your dev DB directlySlackRead threads, summarize discussions, draft responses

Local servers talk over stdio, vendor-hosted ones speak HTTP with OAuth:

```js
1
```
```js
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp
```

Team-shared MCPs land in `.mcp.json` at the project root. Personal ones live in `~/.claude.json`. Once it’s wired, the next prompt you write stops being about your repo and starts being about your whole stack.

### 9.1 A Real Obsidian Workflow

Obsidian and Claude Code click when you treat the vault as three tiers of memory. Skip that framing and you’re back to “Claude can read my files,” which misses the point.

**Setup:** Install `obsidian-claude-code-mcp` in Obsidian. It exposes the vault over a local WebSocket on port 22360, and Claude Code finds it on its own. Drop a `CLAUDE.md` at the vault root so the agent knows your folder layout.

**Folder structure:**

```js
1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
```
```js
vault/
├── 00-Inbox/         # raw capture
├── 10-Daily/         # one note per day
├── 20-Projects/      # active project notes
│   └── billing-v2/
│       ├── README.md      # goal, status, open questions
│       ├── decisions/     # ADRs
│       └── sessions/      # one log per Claude session
├── 30-Decisions/     # cross-project ADRs
├── 40-Atoms/         # reusable knowledge, linked
└── 90-Archive/
```

**The three tiers:**

**Hot storage: daily session log.** Every session I run writes a timestamped entry into `10-Daily/<today>.md`. I wire a `Stop` hook so the append happens when the session ends. No copy paste.

**Warm storage: project notes.** Each project lives under `20-Projects/`. When I start a session, Claude reads the project README and the last two or three session logs before touching anything. Two weeks of context rehydrated in about 30 seconds.

**Cold storage: decisions and atoms.** Architectural calls get promoted into `30-Decisions/` as ADRs once they’ve stuck. Reusable knowledge gets distilled down into `40-Atoms/` with wikilinks so the same fact threads across every project that needs it.

**Daily workflows:**

- `What is in my inbox? Summarize and suggest where each item belongs.`
- `Check 30-Decisions/ for anything related to retry policies.`
- `Read the last 3 session logs for billing-v2. Tell me where I left off.`

> **:** Resist installing every MCP. Each one expands the tool list Claude reasons over, and bloated tool lists hurt decision quality. Starter set: GitHub, Context7, and one or two domain-specific.

> **:** Run `/mcp` inside Claude Code to list every active server and its connection status. First place to check when something isn’t working.

## 10\. Optimizing Your Daily Workflow

**Morning.** Open Claude Code in the project. Skim whatever the subagents and scheduled jobs churned through overnight. Once a week, run `/insights` and actually read it.

**New feature.** Start in plan mode, then edit the plan with `Ctrl+G` until it matches your head. Implement. Either invoke the `/pr-review` subagent or fire up a fresh Claude session to tear it apart.

**Bug.** Reproduce it before you touch anything. Pipe the error in with `cat error.log | claude` and ask Claude to write a failing test that reproduces the bug. Only after that test goes red do you ask for a fix. Skip this step and the fix is just a guess wearing a suit.

**Migrations or mass changes.** Reach for `/batch`. It interviews you about what you actually want, then fans out to parallel agents, each in its own worktree, each running tests and opening its own PR. You become a reviewer instead of a typist.

**Unfamiliar code.** Hand it to a subagent. Something like, “Use a subagent to investigate how our auth handles token refresh.” It chews through dozens of files inside its own context window and reports back with a tidy summary. Your main session stays uncluttered, which matters more than people realise once they’ve burned a 200k token window on spelunking.

**Parallel sessions.** Boris and the team call this the single biggest productivity unlock, and I agree only after kicking against it for a week. Three to five git worktrees, each running its own Claude session. Use the agent view (`claude agents`) as a control plane so you can see who’s doing what without alt-tabbing through six terminals.

**Writer/Reviewer pattern.** Session A implements the change. Session B reviews it in a completely fresh context, no prior conversation baggage. Copy the review back into Session A, fix, repeat until Session B stops complaining.

**Compact at milestones.** After you finish a logical chunk, run `/compact Preserve the decisions made, files changed, and test commands.` Do it before the context gets soupy, do it kindly, do it often.

> **:** Never let Claude claim success without evidence, whether that’s tests, screenshots, or real command output. The trust-then-verify gap is the single biggest source of bad output.

## 11\. Tips From the Anthropic Team

The difference between people who get Claude and people who fight it comes down to maybe a dozen habits. Here’s what Boris, Cat Wu, Thariq, and the rest of the team actually do day to day…

**“Give Claude a way to verify its output. Once you do that, Claude will iterate until the result is great.”** Boris’s single most-repeated point.

**Use Opus with high or xhigh effort for almost everything.** A smaller model that needs more correction ends up slower overall. That’s why Boris defaults to Opus.

**Run 3-5 sessions in parallel.** Worktrees beat checkouts. Use `claude --worktree` or the Desktop app. The agent view ties them together.

**Keep a notes directory per project, updated after every PR.** Tell Claude to write notes into a directory and point `CLAUDE.md` at it. Your codebase compounds in self-knowledge.

**Build a `/techdebt` slash command.** Run it at the end of every session to find and kill duplicated code.

**The team’s `CLAUDE.md` is shared and edited multiple times a week.** Whenever someone watches Claude get something wrong, they add a rule. Treat it as a living document.

**`Esc` twice opens rewind.** Pair it with checkpoints, try something risky, find out it failed, rewind cleanly.

**For UI changes, set up Playwright MCP.** Boris reaches for the Chrome extension every time he touches web code. Claude opens a browser, clicks around, and verifies the result.

**Install a language server plugin.** You’ll catch type errors and unused imports after every edit. Highest-impact plugin you can install.

**Use `/voice` for prompting.** You speak 3x faster than you type, and your prompts get way more detailed once you do.

**Auto mode + `/focus` + `/goal`.** Write a crisp brief, set the goal, walk away. Come back to a finished PR.

**Use `Ctrl+G` to edit Claude’s plan in your editor before implementation.** Faster than typing corrections into the chat.

**Ask Claude to draw ASCII diagrams of new protocols and codebases.** Boris’s trick for understanding unfamiliar code fast.

## 12\. Resources

**Official docs**

**Boris and the team**

- [How Boris Uses Claude Code](https://howborisusesclaudecode.com/): 89+ s straight from the creator, pulled from his X threads
- [Anthropic blog: Best practices for Opus 4.7 with Claude Code](https://claude.com/blog/best-practices-for-using-claude-opus-4-7-with-claude-code)
- [shanraisshan/claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice)

**Skills**

- [mattpocock/skills](https://github.com/mattpocock/skills): “Skills for Real Engineers”
- [Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills): 66 language-specific skills
- [addyosmani/web-quality-skills](https://github.com/addyosmani/web-quality-skills): web performance and quality
- [Anthropic skills cookbook](https://platform.claude.com/cookbook/skills-notebooks-01-skills-introduction)

**Subagents**

- [VoltAgent/awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents): 100+ sorted by category
- [hesreallyhim/a-list-of-claude-code-agents](https://github.com/hesreallyhim/a-list-of-claude-code-agents)

**Plugins and marketplaces**

- [Chat2AnyLLM/awesome-claude-plugins](https://github.com/Chat2AnyLLM/awesome-claude-plugins): 1000+ plugins across 75+ marketplaces
- [claudemarketplaces.com](https://claudemarketplaces.com/)

**MCPs**

- [Obsidian Claude Code MCP plugin](https://github.com/iansinnott/obsidian-claude-code-mcp)
- [Official MCP servers list](https://github.com/modelcontextprotocol/servers)
- [claude.com/partners/mcp](https://claude.com/partners/mcp)

## Closing Notes

Claude Code clicked for me once I quit treating it like `ChatGPT` in a terminal. The mental model flipped from “I need to write this code” to “I need to set Claude up to write this code well.” Setup is the work. Execution is verification.

A few things that have genuinely changed how I work:

**`CLAUDE.md` is compounding infrastructure.** Every mistake Claude makes is a rule waiting to be written. After a few weeks of “update CLAUDE.md so you don’t repeat this,” the same prompts produce dramatically better output.

**`CLAUDE.local.md` captures PR feedback.** Your reviewers are handing you free training data. Convert recurring comments into rules. Let Claude apply them next round.

**Skills are the unit of reusable expertise.** If you’ve prompted the same instructions twice, you’ve got a skill waiting to be written.

**Subagents over kitchen-sink prompts.** Separate the concerns, keep each context clean, and per-task quality goes up.

**Parallel sessions are the unlock everyone underestimates.** Three Claudes in three worktrees is a different kind of force multiplier. Try it for a day.

Most people stop at the prompts. The real value sits past that, in the directory layout, skills, subagents, plugins, and MCPs. You train it, configure it, operate it. The output tracks the configuration.

These are my personal takeaways from how I’ve used Claude Code day to day, so your setup and mileage will look different. If you spot something off or have a sharper way to do any of this, feel free to [reach out](mailto:arpanpatel.contact@gmail.com?subject=Claude%20Code%20Mastery), I’d love to hear it:)

Claude Code icon by [LobeHub](https://lobehub.com/), used under the [Apache 2.0 license](https://github.com/lobehub/lobe-icons/blob/master/LICENSE).