---
title: "Orchestrating Coding Agents"
source: "https://www.instapaper.com/read/2000215052"
author:
  - "[[Melty Labs]]"
published: 61 min
created: 2026-07-24
---
[talks.addy.ie](https://talks.addy.ie/oreilly-codecon-march-2026/)

## ORCHESTRATINGCODING AGENTS

Patterns for Coordinating Agents in Real-World Software Workflows

Addy Osmani

Welcome everyone. I'm Addy Osmani, and for the next twenty minutes we'll cover the broader landscape of patterns and tools for orchestrating coding agents. We'll go from subagents to full agent teams to quality gates that make these systems trustworthy. If you saw Ryan Carson's talk on Antfarm and the Ralph loop, we'll build on those ideas. ═══════ EXTENDED NOTES ═══════ Welcome everyone. If you were here for Ryan Carson's talk, you just saw how to set up an agent team in OpenClaw with one command using Antfarm - that's one powerful way to do multi-agent orchestration. I'm going to zoom out and cover the broader landscape of patterns and tools for orchestrating coding agents, including some that complement what Ryan showed. I'm Addy Osmani, and for the next twenty minutes we'll go from subagents to full agent teams to quality gates that make these systems trustworthy. A lot of what Ryan covered with the Ralph loop and shipping while you sleep will come back later in my talk - we'll build on those ideas.

Quick intro - Director at Google, Gemini and Cloud AI. Pro-tip: everything I show today with Claude and Claude Code is available through Google Cloud's Vertex AI with your existing billing and compliance setup. Find me on X at addyosmani and on LinkedIn. Let's get into it. ═══════ EXTENDED NOTES ═══════ Quick intro - I'm Addy Osmani, I'm a Director at Google working on Gemini and Cloud AI. Quick pro-tip before we dive in: everything I show today with Claude and Claude Code is available through Google Cloud's Vertex AI. We offer Anthropic's models alongside Gemini and hundreds of other models. So if your company is already on Google Cloud, you can use Claude Code through Vertex AI with your existing billing and compliance setup. You can find me on X at addyosmani and on LinkedIn. Let's get into it.

THE MOMENT

## You used to pair with one AI.Now you manage an agent team.

BEFORE

One AI + One Developer

Sequential | Synchronous

Context window is your ceiling

NOW

Multiple Agents + One Orchestrator

Parallel | Asynchronous

The codebase is your canvas

The moment we're in. Before: one developer, one AI, synchronous, context window is your ceiling. Now: multiple agents running asynchronously, each with its own context window, while you orchestrate from above. The codebase becomes your canvas, not a conversation thread. This talk is about how to make that transition. ═══════ EXTENDED NOTES ═══════ Here's the moment we're in. On the left is how most of us started with AI coding tools: one developer, one AI, taking turns. You wait for it, it waits for you. Synchronous. And your ceiling is whatever fits in that single context window. On the right is where we're heading - and where some teams already are. Multiple agents running asynchronously, each with its own context window, while you orchestrate from above. The codebase becomes your canvas, not a conversation thread. This talk is about how to make that transition.

WHERE ARE YOU?

## The 8 Levels of AI-Assisted Coding

Adapted from Steve Yegge's developer evolution framework

AI AUTONOMY + TRUST ↑

L8

Build your own orchestrator

You write the coordination layer yourself, spawning, routing, and managing agents programmatically.

L7

10+ agents, managed by hand

Wrong context sent to the wrong agent. You start asking: "What if Claude Code could run Claude Code?"

L6

Agent multiplexing

Bored waiting? Fire up another agent. Then another. You're bouncing between streams and you can't stop.

L5

Agent-first, IDE later

You work in the agent conversation, the IDE is just where you look at the code afterward.

L4

Diffs fade, conversation leads

You stop reviewing every diff, you watch what the agent is doing and focus on guiding it.

L3

YOLO mode

Agent runs freely in IDE, trust is rising.

L2

Agent in IDE, permissions on

You approve every file change, full manual control.

L1

No AI

Traditional dev workflow, no AI tooling.

ORCHESTRATION

This talk covers  
L6 – L8

AGENT-FIRST

Most devs should  
reach L5 soon

IDE ERA

Eight levels of AI-assisted coding, bottom to top. IDE Era: L1 no AI, L2 agent with permissions, L3 YOLO mode, L4 conversation leads over diffs. Agent-First: L5 is the inflection point where most devs should aim now, L6 is multiplexing multiple agents. Orchestration: L7 managing 10+ agents by hand, L8 building your own orchestration layer. This talk covers L5 through L8. Where are you on this ladder? ═══════ EXTENDED NOTES ═══════ Steve Yegge recently outlined eight levels of how developers evolve with AI tools, and I think it's a powerful framing. Read it bottom to top - it's a ladder of autonomy and trust. At the bottom, the IDE Era. L1 is no AI at all. L2, you've got an agent in your IDE but you're approving every file change. L3 is YOLO mode - you let it run freely and trust is growing. L4, you stop reviewing every diff and start guiding the conversation instead. Now the middle tier: Agent-First. L5 is the inflection point - the agent conversation is primary, the IDE is just where you inspect the output. Most developers should be aiming to reach L5 in the next few months. L6 is agent multiplexing - you get bored waiting for one, so you fire up another, then another. You're bouncing between streams. And then the top tier: Orchestration. L7, you're managing ten-plus agents by hand and it's getting messy. You start asking "what if Claude Code could run Claude Code?" L8 is the endgame - you build your own orchestration layer, spawning and routing agents programmatically. This talk covers L5 through L8. Where are you on this ladder right now?

## THE SHIFT: CONDUCTOR TO ORCHESTRATOR

### CONDUCTOR MODEL

Developer

▼

AI Agent

Single agent, real-time guidance  
Synchronous - you wait for it  
Context window = hard ceiling  
Sequential task execution  
Tools: Claude Code CLI, Cursor in-editor

### ORCHESTRATOR MODEL

Orchestrator

▼ ▼ ▼

Agent 1

Agent 2

Agent 3

Multiple agents, async execution  
Asynchronous - they work while you plan  
Multiple context windows in parallel  
True concurrent task execution  
Tools: Agent Teams, Conductor, Codex, Copilot

The core mental model shift. Conductor model: one agent, synchronous, context window is your ceiling. Orchestrator model: multiple agents with their own context windows, working asynchronously while you plan and check in. It's the difference between pair programming and managing a team. You need different skills now: clear specs, work decomposition, and output verification rather than writing code yourself. ═══════ EXTENDED NOTES ═══════ Let me frame the core mental model shift. On the left is the conductor model - what we've been doing. You're the conductor, there's one musician, and you guide them in real time. It's synchronous, sequential, and your context window is a hard ceiling. On the right is the orchestrator model. You're coordinating an entire ensemble. Multiple agents, each with their own context window, working asynchronously. You plan the work, assign it, and check in periodically. This is the difference between pair programming and managing a team. And just like managing a team, you need different skills - you need to write clear specs, decompose work, and verify output rather than writing code yourself.

## THE SINGLE-AGENT CEILING

Three walls every developer hits when working with one AI at a time

### Context Overload

One agent can only hold so much. Large codebases overwhelm a single context window. You lose important details as the conversation grows.

### No Specialization

One agent does everything: data layer, API, UI, tests. Jack of all trades, master of none. Focused agents produce better code.

### No Coordination

Even if you spawn helpers, they can't communicate, share a task list, or resolve dependencies. The more agents, the harder it gets.

Why can't one agent do it all? Three walls. Context overload - large codebases overwhelm a single context window. No specialization - a focused data-layer agent writes better database code than a generalist. No coordination - multiple agents can't share task lists or resolve dependencies automatically. Subagents solve the first two; Agent Teams solve coordination. ═══════ EXTENDED NOTES ═══════ So why can't we just use one really good agent? Three walls. First, context overload. One agent can only hold so much information. When you're working on a large codebase, a single context window gets overwhelmed and starts losing important details. Second, no specialization. When one agent does everything - data layer, API, UI, tests - it's a jack of all trades and master of none. A focused agent that only handles the data layer writes significantly better data code. Third, and this is the subtle one: no coordination. Even if you split work across multiple agents, they can't talk to each other, can't share a task list, and can't automatically resolve dependencies. The more agents you add, the harder coordination gets. These three constraints create a progression: subagents solve the first two - focused context and specialization. But Agent Teams are what you need when coordination becomes the bottleneck.

## WHY MULTI-AGENT?

3x

### Parallelism

3 agents build frontend, backend, and tests simultaneously

Focused

### Specialization

Each agent has focused context - only the files it owns

Safe

### Isolation

Git worktrees prevent merge conflicts between agents

Growing

### Compound Learning

AGENTS.md accumulates patterns across sessions

Why multi-agent? Four compounding reasons. Parallelism: 3x throughput. Specialization: focused agents write better code. Isolation: git worktrees mean no merge conflicts. Compound learning: AGENTS.md accumulates patterns across sessions. These benefits don't just add up - they multiply. ═══════ EXTENDED NOTES ═══════ So why multi-agent? Four reasons, and they compound. First, parallelism. Three agents building frontend, backend, and tests simultaneously means roughly three-x throughput. Second, specialization. Each agent only sees the files it owns. An agent that only knows about db.js writes better database code than one juggling your entire codebase. Third, isolation. Git worktrees give each agent its own working directory, which means no merge conflicts while they work. And fourth, compound learning. An AGENTS.md file accumulates patterns and gotchas across sessions, so every session makes the next one better. These four benefits don't just add up - they multiply.

PATTERN 1

## Subagents: Focused Delegation

Parent agent spawns specialized child agents - can run in parallel when independent

Parent Orchestrator

▼ ▼

PHASE 1 - PARALLEL

**Data Layer**

db.js - schema, CRUD

**Business Logic**

validation.js - rules

DATA.md LOGIC.md

→

PHASE 2 - DEPENDS ON BOTH

**API Routes**

server.js - Express

API.md

✓ **Parallel** when tasks are independent

✓ Focused context per agent

⚠ Parent manages dependencies manually

⚠ No peer messaging or task list

Cost-neutral (~220k tokens) | Report files bridge context | Runs within a single Claude Code session

Pattern One: subagents - the simplest multi-agent pattern, start here. Key insight: subagents CAN run in parallel. The parent spawns independent tasks simultaneously - data layer and business logic run concurrently, both write report files, then API routes reads both reports and wires everything up. Parent manages dependencies manually. Works for simple cases but no shared task list, no peer messaging, no automatic dependency resolution. That's the gap Agent Teams fill. ═══════ EXTENDED NOTES ═══════ Let's start with Pattern One: subagents. This is the simplest multi-agent pattern and the one I recommend you try first. Here's the key insight most people miss: subagents CAN run in parallel. The parent orchestrator uses the Task tool to spawn multiple child agents simultaneously when their tasks are independent. In this example, the data layer subagent and the business logic subagent have no dependency on each other - the data layer builds the SQLite schema and CRUD operations while validation builds input rules at the same time. Both write report files when done. Then the API routes subagent starts - it depends on both, so it reads DATA.md and LOGIC.md to understand the interfaces before wiring up Express. The parent manages these dependencies manually - it knows which agents can run concurrently and which need to wait. That works fine for simple cases but doesn't scale. There's no shared task list, no peer messaging between agents, no automatic dependency resolution. The parent is doing all the coordination work. That's the gap Agent Teams will fill.

DEMO 1

## Subagents Build Link Shelf

```js
🍧  ››› ./run.sh
```

Watch: Parent decomposes >>> spawns Data + Validation in parallel >>> then API Routes reads both reports >>> tests pass

Let's see this in action. We give Claude Code a single prompt: "Build a bookmarks manager called Link Shelf using Express and SQLite." Watch what happens. The parent orchestrator reads the prompt and decomposes it into three subagent briefs. Then - and this is the important part - it spawns the data layer and business logic subagents simultaneously. They're independent tasks with no shared dependencies: db.js builds the schema and CRUD operations while validation.js builds input rules. Both run at the same time. When both finish and write their report files, the parent spawns the third subagent - API routes - which reads both reports to understand the interfaces and wires everything together in server.js. So we get parallelism where possible, but the parent is manually managing the dependency graph. It works, but notice what's missing: no shared task list, no peer messaging, no automatic unblocking. The parent is doing all the orchestration logic itself. That's exactly the gap Agent Teams will fill in Demo 2.

## SUBAGENTS: KEY TAKEAWAYS

### ✓ What subagents solve

**Context overload** - each agent only sees its own files  
**Specialization** - focused agents write better code  
**Parallelism** - independent tasks run simultaneously  
**Cost-neutral** - ~220k tokens total, about 2 API calls

### ⚠ What's still missing

**Manual dependency management** - parent must know the graph  
**No peer messaging** - agents can't talk to each other  
**No shared task list** - parent tracks everything itself  
**No file locking** - risk of conflicts if scoping is sloppy

**Subagents = parallel execution, manual coordination** — great for simple decomposition, but the parent is doing all the orchestration work. Agent Teams add the coordination primitives.

Subagents scorecard. What they solve: context isolation per agent, specialization produces better code, parallel execution, cost-neutral at ~220k tokens. What's missing: manual dependency management, no peer messaging between agents, no shared task list, no file locking. Bottom line: subagents give parallel execution with manual coordination. Agent Teams add the missing primitives - shared tasks, messaging, dependency resolution, file locking. ═══════ EXTENDED NOTES ═══════ Here's the honest scorecard for subagents. On the left, what they solve. Context overload - each agent only sees its own files, so it stays focused. Specialization - a data layer agent writes better database code than a generalist. Parallelism - independent subagents run simultaneously, which is faster than sequential. And it's cost-neutral at about 220k tokens total. On the right, what's still missing. The parent has to manually manage the dependency graph - it has to know that API routes depends on both data layer and validation. Agents can't message each other directly - if the data layer agent discovers something the validation agent needs to know, there's no communication channel. There's no shared task list, so the parent is tracking everything in its own context. And there's no file locking - if you're sloppy about scoping, two agents could write to the same file. The summary: subagents give you parallel execution with manual coordination. That's great for simple decomposition. But Agent Teams add the coordination primitives - shared task list, peer messaging, automatic dependency resolution, file locking - that make complex parallel work manageable.

PRO-TIP 1

## Hierarchical Subagents: Teams of Teams

Don't stop at one level of delegation. Feature leads that spawn their own specialists.

Parent Orchestrator

▼ ▼

Feature Lead A

▼ ▼ ▼

Data

Logic

API

Feature Lead B

▼ ▼ ▼

UI

State

Tests

### How It Works

Parent gives Feature Lead A a brief: "Build the search feature." Feature Lead A decomposes it further into Data, Logic, API subagents - each with focused context. Parent never sees the details.

### Why It Works

3x deeper decomposition without exploding the parent's context window. Each layer only holds the context it needs. Mimics how real engineering orgs delegate through layers of leads.

Advanced pro-tip: hierarchical subagents - teams of teams. Instead of spawning six subagents, spawn two feature leads. Each lead spawns its own specialists. Parent only talks to two agents, keeping context clean. Feature Lead A decomposes "build search" into Data, Logic, API subagents on its own. 3x deeper decomposition without exploding anyone's context window. Mimics how real engineering orgs delegate through layers of leads. ═══════ EXTENDED NOTES ═══════ Before we move to Agent Teams, here's an advanced pro-tip: hierarchical subagents, or teams of teams. Don't stop at one level of delegation. Instead of your orchestrator spawning six subagents - which fragments its context - spawn two feature leads. Each feature lead then spawns its own two or three specialists. The parent orchestrator only talks to two agents, keeping its context clean. Feature Lead A gets a brief like "Build the search feature" and decomposes it into Data, Logic, and API subagents on its own. The parent never sees those details. This gives you three-x deeper decomposition without exploding anyone's context window. It mimics how real engineering organizations work - you don't have the VP of Engineering assigning tasks to individual engineers. You go through layers of tech leads.

PATTERN 2

## Agent Teams: True Parallel Execution

Team Lead

▼

SHARED TASK LIST

pending | in\_progress | completed — Dependency tracking | File locking

▼ ▼ ▼

Backend  
Teammate

← peer msg →

Frontend  
Teammate

← peer msg →

Test  
Teammate

Each teammate = independent Claude Code instance with own context window  
Teammates self-claim tasks | Direct peer messaging | Lead synthesizes results | Runs in tmux split panes

Pattern Two: Agent Teams - true parallel execution. Enable with CLAUDE\_CODE\_EXPERIMENTAL\_AGENT\_TEAMS=1. Architecture: Team Lead on top, shared task list with dependency tracking and file locking, then teammates below. Each teammate is an independent Claude Code instance with its own context window. They self-claim tasks, message each other peer-to-peer, and run in tmux split panes so you can watch them all at once. ═══════ EXTENDED NOTES ═══════ Now let's talk about Pattern Two: Agent Teams. This is where things get really exciting. Agent Teams are Claude Code's experimental feature for true parallel execution. You enable it with the environment variable CLAUDE\_CODE\_EXPERIMENTAL\_AGENT\_TEAMS equals one. Here's the architecture. You have a Team Lead at the top. Below that is a shared task list - tasks with statuses like pending, in progress, and completed, plus dependency tracking and file locking. Below the task list, you have your teammates - in this example, a backend teammate, a frontend teammate, and a test teammate. Each teammate is a fully independent Claude Code instance with its own context window. They self-claim tasks from the shared list, they message each other directly - peer-to-peer, not through the lead - and the lead synthesizes results when everyone's done. They run in tmux split panes so you can watch them all at once.

## HOW AGENT TEAMS WORK

### SHARED TASK LIST

pending Add search API endpoint

in\_progress Build search UI component

completed Set up database schema

blocked Write search tests (depends on API)

Press Ctrl+T to toggle task view  
File locking prevents race conditions

### COMMUNICATION

Lead >>> Backend:

Build GET /api/links/search with LIKE query

Backend >>> Frontend:

API contract: GET /search?q= returns \[{id,title,url}\]

Frontend >>> Lead:

Search UI complete, wired to API, debounced input

Lead >>> All:

All tasks done. Running integration tests.

Direct peer messaging between teammates  
Idle notifications auto-delivered to lead

Two mechanisms make Agent Teams work. Shared task list: tasks have statuses (pending, in progress, completed, blocked) with dependency tracking - blocked tasks auto-unblock when dependencies complete. Control-T toggles the visual overlay. File locking prevents conflicts. Communication: teammates message each other directly - backend sends frontend the API contract without going through the lead. Peer-to-peer messaging prevents the lead from becoming a bottleneck. ═══════ EXTENDED NOTES ═══════ Two mechanisms make Agent Teams work. On the left, the shared task list. Each task has a status: pending, in progress, completed, or blocked. Blocked tasks have explicit dependencies - like "write search tests" is blocked until the search API is done. When the backend teammate finishes the API endpoint and marks that task completed, the blocked test task automatically unblocks. You can press Control-T at any time to toggle a visual overlay of the task list. File locking prevents two teammates from editing the same file simultaneously. On the right, the communication system. This is critical. Teammates message each other directly. The backend agent tells the frontend agent the API contract - "GET /search?q= returns an array of id, title, url" - without going through the lead. When a teammate goes idle, the lead is automatically notified. This peer-to-peer messaging is what prevents the lead from becoming a bottleneck.

DEMO 2 - HERO DEMO

## Agent Teams: 3 Teammates Build Search in Parallel

Watch: Lead spawns team >>> Backend + Frontend + Tests work simultaneously >>> dependencies auto-resolve

This is the hero demo of the talk. We start with our working Link Shelf bookmarks app and give Claude Code one prompt: "Create a three-person agent team to add search functionality." Watch what happens. The lead analyzes the codebase, creates a task list with dependencies, and spawns three teammates. The backend teammate starts building the search API endpoint. The frontend teammate starts building the search UI component with a debounced input. The test teammate is initially blocked - waiting for the API to be done. When the backend teammate finishes and messages the API contract to the frontend teammate, the test task automatically unblocks, and all three are working simultaneously. Watch the tmux panes - you'll see code being written in parallel across all three.

DEMO 3

## Agent Team Communication Up Close

Watch: Ctrl+T task list >>> teammate messages >>> dependency auto-unblock >>> lead synthesis

This shorter demo zooms in on the communication mechanisms. I want you to watch three things. First, press Control-T and see the task list overlay - you can see which tasks are pending, which are in progress, which are blocked and why. Second, watch the dependency auto-unblock. When the backend teammate marks the API endpoint as completed, the blocked test-writing task flips to pending and the test teammate picks it up immediately. Third, notice the peer messages. The backend agent sends the frontend agent the API contract directly - no lead needed as an intermediary. And at the end, the lead synthesizes all the work into a final integration.

## AGENT TEAMS: KEY TAKEAWAYS

### True parallelism with coordination

Not just running things at the same time - shared task list with dependency tracking ensures work happens in the right order.

### Peer messaging prevents bottlenecks

Teammates communicate directly. Backend tells Frontend the API contract without the lead as intermediary.

### Plan approval for risky tasks

Require teammates to plan before implementing. Lead reviews and approves/rejects - catching issues before code is written.

### Right-size your team

3-5 teammates is the sweet spot. Token costs scale linearly. Three focused teammates outperform five scattered ones.

Four Agent Teams takeaways. True parallelism WITH coordination - shared task list ensures right order. Peer messaging prevents bottlenecks - teammates communicate directly, no lead intermediary. Plan approval for risky tasks catches architectural problems before code exists. Right-size your team: 3-5 teammates is the sweet spot - three focused teammates outperform five scattered ones. ═══════ EXTENDED NOTES ═══════ Four takeaways from Agent Teams. First, this is true parallelism with coordination. It's not just running things at the same time - the shared task list with dependency tracking ensures work happens in the right order. Second, peer messaging prevents bottlenecks. Teammates communicate directly. The backend agent tells the frontend agent the API contract without waiting for the lead. This is huge for velocity. Third, use plan approval for risky tasks. You can require teammates to write a plan before implementing. The lead reviews and approves or rejects - catching architectural problems before code exists. And fourth, right-size your team. Three to five teammates is the sweet spot. Token costs scale linearly, and we've found that three focused teammates consistently outperform five scattered ones.

PRO-TIPS 2 – 3

## Making Agent Teams Reliable

### 2\. Loop Guardrails + Reflection Step

Every teammate gets a hard MAX\_ITERATIONS=8. Before each retry, force a 30-second reflection prompt:

"What failed? What specific change  
would fix it? Am I repeating  
the same approach?"

Benchmarks show this substantially cuts stuck agents.

**Why:** Prevents infinite loops, saves tokens, improves problem-solving through forced self-correction.

### 3\. Dedicated @reviewer Teammate

Spawn a permanent @reviewer teammate:

**Model:** Claude Opus 4.6 (read-only)  
**Tools:** lint, test, security-scan only  
**Trigger:** auto on every TaskCompleted  
**Ratio:** 1 reviewer per 3–4 builders

Lead only sees green-reviewed code.

**Why:** Automates quality control without the lead becoming a bottleneck.

Two reliability pro-tips. Loop guardrails: every teammate gets MAX\_ITERATIONS=8, plus a forced reflection prompt before each retry - "What failed? What would fix it? Am I repeating myself?" Cuts stuck agents by 67%. Dedicated @reviewer: Claude Opus 4.6, read-only, auto-triggered on every TaskCompleted. Lead only sees green-reviewed code. One reviewer per 3-4 builders - a permanent CI quality gate built into the team. ═══════ EXTENDED NOTES ═══════ Two advanced pro-tips for making Agent Teams reliable. Pro-tip two: loop guardrails with a reflection step. This is a simple but powerful pattern. Every teammate gets a hard cap of MAX\_ITERATIONS equals eight. But more importantly, before each retry, you force a thirty-second reflection prompt: "What failed? What specific change would fix it? Am I repeating the same approach?" Benchmarks show this single change cuts stuck agents by sixty-seven percent. Without it, agents loop endlessly trying the same broken approach. With it, they self-correct. Pro-tip three: spawn a dedicated reviewer teammate. Give it Claude Opus 4.6, make it read-only with tools limited to lint, test, and security-scan. Every TaskCompleted event automatically triggers it. The lead only ever sees green-reviewed code. The sweet spot is one reviewer per three to four builder agents. It's like having a permanent CI quality gate built into the team itself.

PATTERN 3

## Orchestration at Scale

When you need to manage 5, 10, 20+ agents across multiple repos and features

### Conductor

Melty Labs · macOS

Visual dashboard for Claude Code + Codex agents  
Git worktree isolation per agent  
BYOK - free orchestration layer

### Vibe Kanban

BloopAI · Cross-platform · Open Source

Kanban board for parallel agent tasks  
CLI + web UI, diff review built-in  
Supports Claude, Codex, Gemini, Amp

### Copilot Coding Agent

GitHub · GA for all paid plans

Assign GitHub issue → PR in background  
GitHub Actions powered cloud env  
Self-reviews code before tagging you

### OpenClaw optionally + Antfarm

Open Source · snarktank/antfarm

Agent team in one command (plan → dev → test)  
Ralph loop with YAML + SQLite + crons  
Deterministic workflows, ships while you sleep

### Claude Squad

Open Source

tmux-based multi-agent orchestration  
Git worktree isolation per session  
Terminal UI dashboard - zero setup

### Gastown

Steve Yegge · Open Source

"K8s for AI agents" - 20-30 concurrent  
Mayor/Witness/Polecat/Deacon roles  
Beads: immutable git-backed queryable memory

**Endgame:** Google A2A protocol enables cross-vendor handoff - Claude agents delegate to Copilot/Codex agents via structured JSON.

Pattern Three: orchestration at scale for 5-20+ agents. Six tools to know: Conductor (macOS, visual dashboard, BYOK), Vibe Kanban (cross-platform Kanban board), Copilot Coding Agent (GitHub-native, auto-PR), OpenClaw+Antfarm (Ralph loop, batteries-included), Claude Squad (zero-setup tmux), and Gastown (K8s for agents, 20-30 concurrent). Endgame: A2A protocol enables cross-vendor agent handoff via structured JSON. ═══════ EXTENDED NOTES ═══════ Pattern Three: orchestration at scale. When you need to manage five, ten, or twenty-plus agents across multiple repos and features, you need purpose-built orchestration tools. Let me walk through the updated 2026 landscape. Conductor by Melty Labs - macOS only for now - gives you a visual dashboard with git worktree isolation; it's free, BYOK. Vibe Kanban by BloopAI is the cross-platform alternative: a Kanban board for agent tasks with a CLI plus web UI, built-in diff review, and support for Claude Code, Codex, Gemini, and Amp. Gastown by Steve Yegge is "Kubernetes for AI agents" - twenty to thirty concurrent instances with role-based architecture and Beads for persistent RAG-queryable memory. GitHub Copilot Coding Agent is the enterprise-grade option: assign any GitHub issue to Copilot, it opens a draft PR and works in background via GitHub Actions - it now runs self-review before tagging you. If you saw Ryan Carson's talk earlier - OpenClaw plus Antfarm is the batteries-included agent team that runs the Ralph loop pattern with planner, developer, verifier, tester, and reviewer roles. One command to install, deterministic workflows with YAML and SQLite and crons. It's the most opinionated implementation of "ship while you sleep" out there. Claude Squad is the zero-setup open-source option using tmux and worktrees with a terminal UI dashboard. And Gastown by Steve Yegge is "Kubernetes for AI agents" - twenty to thirty concurrent instances with role-based architecture and Beads for persistent RAG-queryable memory. The endgame is A2A interoperability - agents from different vendors handing off work via structured JSON.

2026 TOOL LANDSCAPE

## Three Tiers of Agentic Coding Tools

Every tool fits one of three operating models - pick the right tier for the job

TIER 1

### In-Process Subagents & Teams - Patterns 1 & 2 in this talk

Claude Code Subagents Claude Code Teams

Single terminal session. Parent spawns children (Task tool) or coordinates a team (CLAUDE\_CODE\_EXPERIMENTAL\_AGENT\_TEAMS=1). No extra tooling needed. Start here.

TIER 2

### Local Orchestrators - Pattern 3 above

Conductor Vibe Kanban Gastown OpenClaw + Antfarm Claude Squad Antigravity Cursor BG Agents

Your machine spawns multiple agents in isolated worktrees. You stay in the loop. Dashboard, diff review, merge control. Best for 3–10 agents on known codebases.

TIER 3

### Cloud Async Agents - NEW paradigm in 2026

Claude Code Web Copilot Coding Agent Jules (Google) Codex Web (OpenAI)

Assign task → close laptop → PR waiting when you return. Agents run in cloud VMs. No terminal, no local setup. The "delegate your backlog while you sleep" tier. Approve plan, review PR, ship.

Most developers in 2026 will use all three tiers - Tier 1 for interactive work, Tier 2 for parallel sprints, Tier 3 to drain the backlog overnight.

Three tiers of agentic tools. Tier 1: in-process subagents and teams in a single session - zero setup, start here. Tier 2: local orchestrators (Conductor, Vibe Kanban, Gastown, etc.) - multiple agents in worktrees, dashboards, best for 3-10 agents. Tier 3: cloud async agents (Claude Code Web, Copilot Agent, Jules, Codex Web) - assign task, close laptop, return to PR. Most devs in 2026 will use all three tiers. ═══════ EXTENDED NOTES ═══════ Let me give you the taxonomy that makes sense of the whole landscape. Three tiers. Tier one is in-process - subagents and Agent Teams running inside a single Claude Code session. This is what we covered in Patterns one and two. No extra tooling required; start here today. Tier two is local orchestrators - tools that run on your machine and spin up multiple agents in isolated worktrees. You stay in the loop with dashboards, diff review, and merge control. Conductor, Vibe Kanban, Gastown, Claude Squad, Antigravity, and Cursor Background Agents all live here. Best for three to ten agents working on a known codebase. Tier three is the paradigm shift: cloud async agents. You assign a task, close your laptop, and come back to a pull request. The agent ran in a cloud VM, understood your codebase, wrote the code, and opened the PR. Claude Code Web, GitHub Copilot Coding Agent, Google Jules, and OpenAI Codex Web all do this. This is the "drain your backlog while you sleep" tier. Most developers in 2026 will use all three tiers depending on what they're doing.

NEW IN 2026

## Cloud Async Agents: Delegate & Walk Away

A new tier of tools where you assign a task, close your laptop, and return to a pull request.

### The Old Model (synchronous)

You watch the agent work.  
You babysit the terminal.  
One task at a time.  
Your attention = the bottleneck.

### The New Model (async cloud)

Describe the task.  
Approve a plan.  
Go do something else.  
PR is waiting when you return.

THE FOUR CLOUD ASYNC TOOLS

Claude Code Web

claude.ai/code · Anthropic cloud VMs · GitHub-native · parallel tasks

Copilot Agent

Assign GitHub issue → GitHub Actions env → PR · GA for all paid plans

Jules

Google · Gemini 2.5/3 Pro · cloud VM · approve plan · async PR

Codex Web

OpenAI · chatgpt.com/codex · GPT-5.4-Codex · parallel cloud tasks

**Common workflow:** Connect GitHub repo → describe task → agent shows plan → approve → PR created → review + merge. Average task: **1–30 min** in cloud VM.

Most important new category in 2026: cloud async agents. Hand off a task, close laptop, return to a PR. Four production tools: Claude Code Web (Anthropic cloud VMs), Copilot Coding Agent (GitHub-native, auto-PR via Actions), Jules (Google, Gemini, plan approval), Codex Web (OpenAI, parallel containers). Same workflow: connect repo, describe task, approve plan, review PR. Average 1-30 minutes. Use this tier to drain your backlog while you sleep. ═══════ EXTENDED NOTES ═══════ Here's the most important new category in 2026: cloud async agents. These are tools where you hand off a task, close your laptop, and come back to a pull request. Four tools now do this at production quality. Claude Code on the web, available at claude.ai/code since October 2025, runs in Anthropic-managed cloud VMs - connect your GitHub repo, describe what you want, and parallel tasks run in isolated environments. GitHub Copilot Coding Agent is GitHub-native - assign any issue to Copilot and it creates a PR via GitHub Actions, runs security scans, and self-reviews before tagging you. Jules is Google's version - powered by Gemini 2.5 and 3 Pro, running in cloud VMs, with a plan approval step before it writes any code. And Codex Web from OpenAI at chatgpt.com/codex runs GPT-5.4-Codex in isolated containers with parallel task execution. They all share the same workflow: connect your repo, describe the task, approve a plan, let the agent run, review the PR. Average task time is one to thirty minutes. This is the tier you use to drain your bug backlog while you sleep.

TOOL SPOTLIGHT Tier 2 · Local Orchestrator

## Conductor by Melty Labs

conductor.build · macOS (Apple Silicon + Intel) · Free, BYOK

WHAT IT DOES

Runs multiple Claude Code *and* Codex agents in parallel on your Mac. Each agent lives in its own isolated Git worktree - a fresh clone of your repo - so agents can never conflict with each other. You get a visual dashboard showing "who's working on what," then review and merge diffs from a single UI.

KEY DIFFERENTIATORS

• **Diff-first review** - see only what changed, not whole files  
• **Free orchestration layer** - pay only your Claude/Codex API costs  
• **Built by ex-Replicate + Netflix ML Infra** (Charlie Holtz, Jackson de Campos)  
• **Now supports Codex** alongside Claude Code

WHEN TO USE IT

3–8 parallel features on same repo · macOS users · BYOK workflow · want diff-first review without leaving your machine. *Not for Linux/Windows yet.*

Conductor by Melty Labs: fastest way to start multi-agent orchestration on Mac. Run Claude Code and Codex agents in parallel, each in its own git worktree. Visual dashboard shows every agent's status, diff-first review UI. Free - you pay only API costs. macOS only; if you're on Linux or Windows, use Vibe Kanban instead. Sweet spot: 3-8 parallel features on the same repo with visual oversight. ═══════ EXTENDED NOTES ═══════ Conductor by Melty Labs is the fastest way to get started with multi-agent orchestration on Mac. The pitch is simple: run a bunch of Claude Code and Codex agents in parallel, each in its own git worktree so they can't conflict. You get a visual dashboard showing every agent's status, and a diff-first review UI so you're only looking at what changed. Critically, it's free - you pay only your Claude or Codex API costs. The orchestration layer itself costs nothing. Built by Charlie Holtz, who previously led growth at Replicate, and Jackson de Campos, who scaled ML infrastructure at Netflix. One caveat: it currently requires macOS with Apple Silicon or Intel - if you're on Linux or Windows, Vibe Kanban is your cross-platform alternative. The use case sweet spot is three to eight parallel features on the same repository where you want visual oversight without setting up complex tooling.

TOOL SPOTLIGHT Tier 1 + Tier 3 · In-Process & Cloud Async

## Claude Code - Teams & Web

Anthropic · code.claude.com + claude.ai/code · Pro/Max/Team/Enterprise

CLAUDE CODE TEAMS (Terminal, Tier 1)

Enable: `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`

✓One Team Lead coordinates shared task list

✓Teammates: fully independent sessions + own context

✓Direct peer-to-peer messaging (no lead bottleneck)

✓Dependency tracking - blocked tasks auto-unblock

✓Plan approval gate before risky tasks

⚠Experimental - tokens scale with team size (3–5 sweet spot)

CLAUDE CODE WEB (Browser, Tier 3)

Launched Oct 2025 · claude.ai/code · No terminal needed

✓Connect GitHub repos from browser

✓Multiple tasks in parallel, each isolated Anthropic VM

✓Real-time progress streaming - steer mid-run

✓Auto PR creation + change summaries

✓iOS app available - delegate from your phone

**Best for:** Non-terminal users, PM-driven bug triage, mobile workflows, running tasks while traveling.

Claude Code has two modes. Teams runs in your terminal - team lead coordinates independent teammates with shared task list and peer messaging, 3-5 teammates sweet spot. Web at claude.ai/code is fully browser-based - connect GitHub repos, tasks run in cloud VMs, steer mid-run, auto PR creation, even from iOS. Key mental model: Teams is for working alongside agents; Web is for delegating and walking away. ═══════ EXTENDED NOTES ═══════ Claude Code has two distinct modes that deserve explicit framing. Claude Code Teams, which we covered in Pattern Two, runs in your terminal with a team lead coordinating independent teammates on a shared task list with peer-to-peer messaging. Enable it with the experimental flag and you get true parallelism in a single CLI session. Sweet spot is three to five teammates. Then there's Claude Code on the web, launched in October 2025 at claude.ai/code. This is the Tier Three version - completely browser-based, no terminal required. You connect your GitHub repos, describe tasks, and they run in Anthropic-managed cloud VMs. You can steer mid-run, get automatic PR creation, and access it from your iOS app. This opens Claude Code to non-terminal users like PMs, security teams, or analysts who want to oversee AI-generated pull requests. Same rate limits as the CLI, but zero local setup. The key mental model: Teams is for when you're actively working alongside the agents; Web is for when you're delegating and walking away.

TOOL SPOTLIGHT Tier 3 · Cloud Async Agent

## Jules by Google Labs

jules.google.com · Powered by Gemini 2.5/3 Pro · Free introductory tier · GA Aug 2025

HOW IT WORKS

1

Connect GitHub repo via Jules GitHub App

2

Describe task (or assign GitHub Issue to Jules)

3

Jules generates a **plan** - you review & approve before any code is written

4

Runs in cloud VM - clones repo, installs deps, edits files, runs tests

5

Opens PR with full diff, reasoning, and terminal logs as evidence

AGENTS.md SUPPORT

Jules reads your repo's `AGENTS.md` automatically - your standards, test commands, and coding preferences are inherited with zero extra config.

STANDOUT FEATURES

🔊 **Audio changelogs** - listen to what changed  
🔁 **Interrupt anytime** - redirect mid-task  
📋 **Jules Tools CLI** - pipe GitHub issues directly  
⚡ **Environment Snapshots** - reuse deps for faster runs

Jules is Google's async cloud coding agent, powered by Gemini 2.5 and 3 Pro. Connect a GitHub repo, describe a task, Jules generates a plan you approve before code is written, then runs in a cloud VM and returns a PR with full reasoning and logs. Three distinctive features: audio changelogs, mid-task interruption, and Jules Tools CLI for piping GitHub issues. Auto-reads AGENTS.md. Free tier at 15 tasks per day. ═══════ EXTENDED NOTES ═══════ Jules is Google's async cloud coding agent, powered by Gemini 2.5 and 3 Pro, out of beta since August 2025. The workflow is elegant: connect a GitHub repo, describe a task or assign a GitHub issue directly to Jules, it generates a plan you review before any code is touched, then runs in a cloud VM and returns a PR with full reasoning and terminal logs as evidence. What's clever about Jules is the transparency: you see exactly why it made each decision. It also has a plan approval step that's human-in-the-loop before any code is written - which builds trust. Three things make Jules distinctive: audio changelogs let you listen to what changed; you can interrupt and redirect it mid-task; and the Jules Tools CLI lets you pipe GitHub issues directly to Jules from the command line. It also auto-reads AGENTS.md, so your coding standards transfer with zero extra config. Pricing starts with a free introductory tier at 15 tasks per day.

TOOL SPOTLIGHT Tier 3 · Cloud Async Agent · GitHub-Native

## GitHub Copilot Coding Agent

github.com/features/copilot/agents · GA for all paid Copilot plans · Agents panel on every GitHub page

THE WORKFLOW

2026 UPDATES

✦ **Model picker** - choose fast or frontier per task  
✦ **Built-in security scanning** before PR opens  
✦ **Self-review loop** - catches its own issues first  
✦ **Third-party agents** - Claude + Codex via Agents panel  
✦ **Trigger from Slack, Jira, Linear, Azure Boards**

VS COPILOT AGENT MODE

**Agent mode** (IDE) = interactive, synchronous, you watch it work.  
**Coding Agent** (GitHub) = async, cloud, opens PR, you review when done. Different tools for different flows.

Key distinction: Copilot agent mode (IDE, synchronous) vs Coding Agent (GitHub, fully async). Assign an issue or use the Agents panel, it creates a draft PR via GitHub Actions. 2026 updates: model picker per task, self-review loop before tagging you, third-party agents (Claude, Codex) in same panel, triggers from Slack/Jira/Linear. It's becoming the GitHub-native orchestration layer. ═══════ EXTENDED NOTES ═══════ GitHub Copilot Coding Agent is important to distinguish from Copilot agent mode in the IDE - they're genuinely different products. Agent mode in VS Code is synchronous, interactive, you watch it work. The Copilot Coding Agent on GitHub is fully async: you assign a GitHub issue to Copilot, or use the Agents panel available on every page on GitHub, and it opens a draft PR and works in the background via GitHub Actions. You're not watching anything; you're notified when it's done. The February 2026 updates made this much more capable: there's now a model picker so you can choose a faster model for simple tasks or a frontier model for gnarly refactors. The self-review loop is huge - it runs Copilot code review on its own output before tagging you, so by the time you're reviewing, someone already went through it. And now third-party agents including Claude Code and Codex are accessible through the same Agents panel, which is genuinely useful for A2A-style workflows. You can also trigger it from Slack, Jira, Linear, and Azure Boards - it's becoming the GitHub-native orchestration layer.

TOOL SPOTLIGHT Tier 2 · Local Orchestrator · Cross-Platform · Open Source

## Vibe Kanban by BloopAI

vibekanban.com · github.com/BloopAI/vibe-kanban · npx vibe-kanban · Free (BYOK)

THE CORE INSIGHT

Agents create a "doomscrolling gap" - the 2–5 minutes they're working where you have nothing to do. Vibe Kanban eliminates this by running agents in parallel across isolated worktrees, so you're always reviewing while something else is running.

HOW IT WORKS

→Create task cards with detailed prompts

→Drag to "In Progress" - agent spins up in own worktree + branch

→Review line-by-line diffs in the board UI

→Send feedback directly back to the running agent

→Create PR when satisfied

MULTI-AGENT SUPPORT

Works with: **Claude Code, Codex, Gemini CLI, Amp, Cursor Agent CLI** and more. Switch agents per-task without changing your workflow. MCP client + server. Built in Rust + TypeScript.

Vibe Kanban by BloopAI solves the "doomscrolling gap" - those 2-5 minutes waiting for an agent. Run multiple agents in parallel so you're always reviewing while something else runs. Kanban interface: create task cards, drag to "In Progress," each gets its own worktree and branch. Review diffs in-board, send feedback to running agents. Supports Claude Code, Codex, Gemini CLI, Amp, and more. Cross-platform, free, BYOK. ═══════ EXTENDED NOTES ═══════ Vibe Kanban by BloopAI is the cross-platform answer to Conductor - and arguably more flexible. The insight behind it is what Louis Knight-Webb, BloopAI's CEO, called the "doomscrolling gap" - the two to five minutes when an agent is running and you have nothing to do. Vibe Kanban eliminates this by running multiple agents in parallel, so you're always reviewing one while others are running. The interface is a Kanban board: you create task cards with detailed prompts, drag them into "In Progress," and a new branch and worktree is created for each one automatically. You review line-by-line diffs in the board UI, send feedback directly back to the running agent, and create a PR when satisfied. What makes it powerful: it supports Claude Code, Codex, Gemini CLI, Amp, Cursor Agent CLI, and others, so you can mix agents. It's also an MCP server itself - other agents can create tasks and move cards via the MCP protocol. Built in Rust and TypeScript for performance. Zero cost - you just pay your API provider. Available on Mac, Windows, and Linux.

TOOL SPOTLIGHT Tier 3 · Cloud Async Agent

## Codex Web by OpenAI

chatgpt.com/codex · GPT-5.4-Codex · 1.6M+ weekly users · ChatGPT Pro/Business/Enterprise

WHAT IT IS

OpenAI's cloud-based SWE agent. Each task runs in a separate sandboxed container preloaded with your GitHub repository. Multiple tasks run in parallel. Powered by codex-1 (o3-based) → now GPT-5.4-Codex with 1M context window and native computer-use.

THE SURFACE ECOSYSTEM

→ **Codex Web** (chatgpt.com/codex) - cloud tasks in browser

→ **Codex CLI** - open-source terminal agent

→ **Codex App** - macOS desktop, parallel thread management

→ **IDE Extension** - VS Code, Cursor, JetBrains, Xcode

→ **GitHub Integration** - PR reviews + Figma MCP connected

VERIFIABLE EVIDENCE

Every Codex task returns citations of terminal logs and test outputs. You can trace every step. Not just "here's the code" - "here's every command I ran, every test result, every decision." Builds trust at scale.

Codex Web: 1.6M weekly users, cloud SWE agent. Each task in sandboxed container with your repo, parallel execution. Now powered by GPT-5.4-Codex with 1M context. Five surfaces: Web, CLI (open-source), macOS App, IDE extensions, GitHub integration - all connected via ChatGPT account. Key differentiator: verifiable evidence with terminal logs and test output citations for every task. ═══════ EXTENDED NOTES ═══════ OpenAI's Codex Web is one of the most important tools in this space - 1.6 million weekly active users as of March 2026. It launched as a research preview in May 2025 and has evolved rapidly. Each task runs in a separate sandboxed container preloaded with your GitHub repo; the container has internet access disabled for security. Multiple tasks run in parallel. What's changed recently is the model: GPT-5.4-Codex is now the default, with a 1 million token context window and native computer-use capabilities. The surface story is what makes Codex compelling - it's not just a web UI. You have Codex Web in the browser, Codex CLI as an open-source terminal agent, a macOS desktop Codex App for managing parallel thread work, and IDE extensions for VS Code, Cursor, JetBrains, and Xcode. Everything is connected by your ChatGPT account, so you can hand a task off from the web to your local IDE seamlessly. The verifiable evidence feature is notable: every completed task returns citations of terminal logs and test outputs, so you can audit exactly what happened - not just trust the output.

TOOL SPOTLIGHT Tier 2 · Agent-First IDE · Free Public Preview

## Google Antigravity - Agent-First IDE

antigravity.google · Launched Nov 2025 · macOS / Windows / Linux · Free · Gemini 3 Pro default

AGENT MANAGER - THE PRIMARY SURFACE

A birds-eye view across multiple workspaces, overseeing dozens of agents simultaneously. Interact with your codebase primarily through the agent, not through writing code directly.

⌘EToggle between Agent Manager and Editor

✓Manage agents across any workspace from one view

✓Hide, focus, or close editor windows from manager

BROWSER USE + VISUAL ARTIFACTS

CONTEXT & CAVEATS

Built by the ex-Windsurf team (Google acquired Windsurf for $2.4B). Supports Gemini 3 Pro, **Claude Sonnet 4.6**, and GPT-OSS-120B. No MCP support yet. Free in public preview.

Google Antigravity: the most agent-first IDE, free in public preview. Agent Manager is the primary surface - oversee dozens of agents across workspaces, Cmd-E toggles to editor. Three key features: browser-in-the-loop (agent tests its own code in Chrome), visual artifacts (screenshots/video of agent work), and visual feedback (annotate screenshots for targeted agent corrections). Supports Claude and Gemini 3. No MCP yet, but zero cost makes it compelling. ═══════ EXTENDED NOTES ═══════ Google Antigravity is the most agent-first IDE on the market right now and it's completely free in public preview. The core idea: the Agent Manager is the primary surface, not the editor. It's a birds-eye view where you oversee dozens of agents simultaneously across multiple workspaces. You interact with your codebase primarily through agents, not through writing code directly. Hit Command-E to toggle between the Agent Manager and a familiar VS Code-style editor. But their bet is that the manager view will become the primary entry point to all your work as agents get better. Three features that matter for orchestration. First, browser-in-the-loop: the agent can actually use Chrome to navigate to pages and test its own code. Second, visual artifacts: the agent takes screenshots and recordings of its browser work so you can review results asynchronously. Third, and this is the clever part, visual feedback: you can leave comments directly on those screenshots, giving the agent specific targeted feedback about exactly what to change. It's like annotating a design review, except the agent reads your annotations and acts on them. Supports Claude Sonnet 4.6 and Opus 4.6 alongside Gemini 3. No MCP support yet, which limits extensibility, but at zero cost it's compelling for parallel agent work.

TOOL SPOTLIGHT Tier 2 + Tier 3 · Cloud Agents + Glass

## Cursor Cloud Agents + Glass

cursor.com/agents · Cloud Agents + Glass UI · $2B ARR · On-demand usage pricing

CLOUD AGENTS

Same agent fundamentals, but running in isolated cloud VMs instead of your local machine. Unlimited parallel agents, no local connection required. Full VM access - build, test, browser, desktop, MCP servers.

→ **Cursor Web:** cursor.com/agents - any device

→ **Cursor Desktop:** select Cloud in agent dropdown

→ **Slack / Linear:** @cursor to kick off agents

→ **GitHub:** comment @cursor on any PR or issue

→ **API:** programmatic agent creation

→ **PWA:** install as mobile app on iOS/Android

GLASS - THE IDE EVOLVES

Agent management as the primary surface; the editor is one instrument underneath it. Developers reacted immediately: *"Now Cursor feels more like an Agent Orchestrator than an IDE."* Part of a larger pattern - the control plane is becoming the primary experience across the entire ecosystem.

THE BIGGER PATTERN

Conductor, Claude Code Web, Copilot Agent, Jules, Vibe Kanban, now Glass - the same shift keeps showing up: the control plane is becoming the primary surface, and the editor is one instrument underneath. IDEs as we know them are evolving.

Cursor Cloud Agents: same agent fundamentals but in isolated cloud VMs. Unlimited parallel agents, no local connection needed. Kick off from web, desktop, Slack, Linear, GitHub, API, or PWA on mobile. Glass is Cursor's new agent-first interface - agent management is the primary surface, editor underneath. This mirrors a broader pattern across the ecosystem: the control plane is becoming primary, IDEs are evolving. Cursor at $2B ARR signals developers are betting on this direction. ═══════ EXTENDED NOTES ═══════ Cursor's Cloud Agents represent the same agent fundamentals we've been discussing but running in isolated cloud VMs instead of on your local machine. The key advantage: you can run as many agents as you want in parallel, your laptop doesn't need to be connected, and each agent gets its own virtual machine with full capabilities - build, test, browser control, desktop access, and MCP server support. What makes Cloud Agents compelling is the surface area: you can kick off agents from the Cursor Web interface at cursor.com/agents on any device, from the desktop app by selecting Cloud in the agent dropdown, from Slack or Linear using the @cursor command, from GitHub by commenting @cursor on a PR or issue, or via API for programmatic workflows. You can even install cursor.com/agents as a PWA on your phone for a native-feeling mobile experience. Then there's Glass - Cursor's new interface explicitly built to make working with agents clear, intuitive, and in your control. Agent management is the primary experience and the traditional editor is something you reach for when you need to go deeper. The developer reaction was immediate: "Now Cursor feels more like an Agent Orchestrator than an IDE." But Glass is one data point in a much larger pattern. Across Conductor, Claude Code Web, GitHub Copilot Agent, Jules, Vibe Kanban - the same shift keeps showing up. The control plane is becoming the primary surface, and the editor is becoming one of several instruments underneath it. IDEs as we know them may stop being the primary tool for software work, or heavily evolve. Cursor at $2B ARR, revenue doubling in three months, signals that developers are betting on this direction.

## ORCHESTRATION AT SCALE: KEY TAKEAWAYS

### Git worktrees solve merge conflicts

Each agent gets its own worktree - a separate checkout of the repo with its own working directory. No file contention, clean merges. Every tool in 2026 (Conductor, Vibe Kanban, Gastown, Jules, Copilot Agent) does this automatically.

### Dashboards give you visibility

When running 5+ agents, you NEED a dashboard. Conductor (macOS), Vibe Kanban (cross-platform), Antigravity's Manager View, and GitHub's Agents panel all provide real-time views. Pick the one that fits your OS and workflow.

### BYOK keeps costs predictable

Conductor, Vibe Kanban, OpenClaw + Antfarm, Claude Squad - all free orchestration layers. Pay only for tokens. Claude Code Web, Jules, Codex Web, and Copilot Agent have their own pricing but include the infra cost.

### Cloud async = delegate your backlog

Claude Code Web · GitHub Copilot Coding Agent · Jules · Codex Web - four production-grade tools where you assign a task, close your laptop, and return to a PR. Use these for routine tasks while you focus on architecture.

Four scale takeaways for 2026. Git worktrees solve merge conflicts - table stakes now. Dashboards give visibility - Conductor for macOS, Vibe Kanban cross-platform, Antigravity for IDE-centric, GitHub Agents panel for GitHub-native. BYOK keeps costs predictable - free orchestration, pay only tokens. Cloud async agents delegate your backlog - Claude Code Web, Copilot Agent, Jules, Codex Web. Assign task, close laptop, return to PR. This works today. ═══════ EXTENDED NOTES ═══════ Four key takeaways from orchestration at scale - now updated for the full 2026 landscape. First, git worktrees solve merge conflicts - every serious tool now does this automatically; it's table stakes, not a differentiator. Second, dashboards give visibility: when running five-plus agents, pick your dashboard based on your OS - Conductor for macOS, Vibe Kanban for cross-platform, Antigravity's Manager View for IDE-centric work, GitHub's Agents panel for GitHub-native workflows. Third, BYOK keeps costs predictable - the best orchestration tools are free, you pay only for API tokens. And fourth, the new key takeaway: cloud async agents let you delegate your backlog. Claude Code Web, Copilot Coding Agent, Jules, and Codex Web are all production-grade. Assign tasks, close the laptop, return to a PR. This isn't sci-fi - it works today and it's how the highest-leverage developers in 2026 are operating.

PRO-TIPS 4 – 6

## Scaling Smart: Cost, Automation & Knowledge

### 4\. Multi-Model Routing

Match models to tasks: Opus/GPT-5 for planning, Sonnet/Codex-mini for implementation (60–70% cheaper), dedicated security model for review. Add a MODEL\_ROUTING.md file.

Planning → Claude Opus 4.6  
Implementation → Sonnet 4.5  
Review → security-scan model  
Tests → Haiku (fast + cheap)

### 5\. Worktree Lifecycle Scripts

agent-spin \<feature> # worktree + branch + agent  
agent-merge \<feature> # rebase + review + PR  
agent-clean # remove finished worktrees

12 lines of bash. Conductor does this visually.

### 6\. Human-Curated AGENTS.md Only

Research shows LLM-generated AGENTS.md can marginally reduce success rates compared to human-curated ones. Never let agents write to it directly.

Keep <80 lines. Sections:  
STYLE | GOTCHAS | ARCH\_DECISIONS | TEST\_STRATEGY

Three scaling tips. Four: multi-model routing - Opus/GPT-5 for planning, Sonnet/Codex-mini for implementation (60-70% cheaper), security model for review. Create a MODEL\_ROUTING.md. Five: automate worktree lifecycle with three aliases (agent-spin, agent-merge, agent-clean) - 12 lines of bash. Six: human-curated AGENTS.md ONLY. LLM-generated ones reduce success by 3-20%. Keep under 80 lines with clear sections. ═══════ EXTENDED NOTES ═══════ Three more pro-tips for scaling smart. Pro-tip four: multi-model routing. Not every task needs your most expensive model. Route planning and architecture tasks to Claude Opus 4.6 or GPT-5 - those are the high-reasoning tasks. Route implementation to Sonnet 4.5 or Codex-mini, which are sixty to seventy percent cheaper and perfectly capable of writing code from a clear spec. Route review to a dedicated security model. Conductor and Gastown both support this natively. Just create a MODEL\_ROUTING.md file that maps task types to models. Pro-tip five: automate the worktree lifecycle. Three shell aliases - agent-spin creates a worktree, branch, and starts a teammate. Agent-merge auto-rebases, runs the reviewer, and opens a PR with a summary. Agent-clean removes finished worktrees. It's about twelve lines of bash, or Conductor does it visually for you. Pro-tip six, and this is the big one: human-curated AGENTS.md only. Recent research has shown that LLM-generated AGENTS.md files actually reduce success rates by three to twenty percent. Never let an agent write to AGENTS.md directly. The lead must approve every line. Keep it under eighty lines. Structure it with clear sections: style, gotchas, architectural decisions, and test strategy.

## THE DEVELOPER'S NEW ROLE

From implementer to orchestrator - four hats you now wear

### Spec Writer

Define clear task briefs with acceptance criteria. Briefs, not vibes.

### Orchestrator

Decompose work, assign agents, resolve blockers, manage dependencies.

### Quality Gate

Review code, run tests, approve plans, enforce standards before merge.

### Learner

Update AGENTS.md with patterns, refine prompts, accumulate institutional knowledge.

Your developer role has four new hats. Spec Writer: define clear briefs with acceptance criteria - briefs, not vibes. Orchestrator: decompose work, assign agents, resolve blockers. Quality Gate: review code, approve plans, enforce standards before merge. Learner: update AGENTS.md, refine prompts, compound institutional knowledge. The developers who thrive aren't those who write the most code - they manage the production of code most effectively. ═══════ EXTENDED NOTES ═══════ Let me talk about the mindset shift. Your role as a developer has fundamentally changed. You now wear four hats. First, you're a Spec Writer. You define clear task briefs with acceptance criteria. Briefs, not vibes. Vague prompts produce vague code. Second, you're an Orchestrator. You decompose work into parallelizable chunks, assign agents, resolve blockers, and manage dependencies between tasks. Third, you're a Quality Gate. You review code, run tests, approve plans, and enforce standards before anything gets merged. And fourth, you're a Learner. You update AGENTS.md with patterns, refine your prompts based on what worked and what didn't, and accumulate institutional knowledge that makes every future session better. The developers who thrive in this new world aren't the ones who write the most code - they're the ones who manage the production of code most effectively.

## QUALITY GATES: TRUST BUT VERIFY

### Plan Approval

Require teammates to plan before coding. Lead reviews approach, approves or rejects. Catches bad architectures before code exists.

`teammate >>> plan >>> lead review >>> approve/reject >>> implement`

### Hooks

Automated checks on lifecycle events. TeammateIdle: verify tests pass before stopping. TaskCompleted: lint + test gate before marking done.

`task done >>> hook runs npm test >>> pass? allow  |  fail? keep working`

### AGENTS.md

Compound learning across sessions. Discovered patterns, gotchas, style preferences. Every agent reads it. Every session updates it.

`session 1 learns >>> AGENTS.md updated >>> session 2 avoids same mistake`

Three quality gates make agent output trustworthy. Plan approval: teammates plan before coding, lead reviews and approves or rejects - cheaper to fix a bad plan than bad code. Hooks: automated checks on lifecycle events - TeammateIdle verifies tests pass, TaskCompleted runs lint plus tests before marking done. AGENTS.md: compound learning across sessions - patterns, gotchas, style preferences that every agent reads and every session updates. ═══════ EXTENDED NOTES ═══════ Trust but verify. Three quality gates make agent output trustworthy. First, plan approval. You can require teammates to write a plan before they start coding. The lead reviews the approach, approves or rejects. This catches bad architectural decisions before any code is written - it's far cheaper to fix a bad plan than to fix bad code. Second, hooks. These are automated checks on lifecycle events. A TeammateIdle hook verifies all tests pass before allowing an agent to stop working. A TaskCompleted hook runs lint plus tests before marking a task as done. If the hook fails, the agent keeps working until it passes. Third, AGENTS.md for compound learning. This file captures discovered patterns, gotchas, and style preferences across sessions. Every agent reads it at the start of a session, and every session adds to it. Session one learns that your project uses a specific testing pattern, AGENTS.md is updated, and session two avoids the same mistake. This is how institutional knowledge compounds over time.

THE REAL BOTTLENECK

## The bottleneck is no longergeneration. It's verification.

Agents can produce impressive output. The hard part is knowing  
with confidence whether that output is *correct*.

Tests that pass before a change don't guarantee they'll catch regressions *from* the change.  
Agents can write tests that are technically valid but miss the cases that matter.  
Context windows mean agents miss constraints outside their current view.

Until verification catches up with generation, **human review is the safety system**.

The bottleneck is no longer generation - it's verification. Agents produce impressive output fast, but knowing it's correct is the hard part. Tests that pass before a change won't necessarily catch regressions from it. Agents write technically valid tests that miss what matters. Context windows mean agents miss constraints outside their view. Until verification catches up with generation, human review is the safety system. That's why plan approval, hooks, and AGENTS.md exist. ═══════ EXTENDED NOTES ═══════ Here's the insight that reframes everything we've been discussing. The bottleneck is no longer generation. It's verification. Agents can produce impressive output at incredible speed. The challenge is knowing with confidence whether that output is correct. Tests that pass before a change don't mean they'll catch regressions introduced by the change. Agents can write tests that are technically valid but miss the cases that actually matter. UI verification remains brittle. Context window limitations mean agents working on large codebases may miss important constraints that exist outside their current view. And flaky environments, which a single developer encounters as an annoying edge case, become systemic blockers when forty agents hit the same flaky test simultaneously. The factory stalls. Until verification infrastructure catches up with generation capabilities, human review is not optional overhead. It is the safety system. The appropriate response to impressive agent output is not to trust it because it looks good. It is to have the architectural understanding and testing discipline to evaluate it rigorously. This is why everything in the next demo - plan approval, hooks, AGENTS.md - exists.

DEMO 4

## Quality Gates & Self-Improving Agents

Watch: Plan approval flow → hooks run on task completion → AGENTS.md updates with new learnings

This final demo ties everything together. Watch three things. First, the plan approval flow - a teammate proposes adding a favorites feature, writes a plan, and the lead reviews it. The lead spots that the plan doesn't include a database migration step, rejects it, and the teammate revises. Second, watch the hooks fire on task completion. When the teammate finishes implementing, the TaskCompleted hook automatically runs npm test, checks for console.log statements, and validates syntax. The hook catches a forgotten console.log, and the agent fixes it before the task is marked as done. Third, notice the AGENTS.md getting updated at the end. The lead adds a new entry: "Always include ALTER TABLE migration when adding columns to existing tables." That learning carries forward to every future session.

PRO-TIPS 7 – 9

## Making Agents Smarter Over Time

### 7\. Self-Reflection → AGENTS.md Proposals

After every task, force the agent to write a REFLECTION.md: what surprised me, one pattern to add, one prompt improvement. The lead reviews and merges approved learnings into AGENTS.md via a quality gate.

**Why:** This is how compound learning actually compounds - systematically, not ad hoc.

### 8\. Token Budgeting & Kill Criteria

Hard per-agent caps: Frontend 180k, Backend 280k tokens. At 85% → auto-pause + notify lead. If stuck 3+ iterations on the same error → kill and reassign.

**Why:** Prevents runaway costs; forces agents to be concise.

### 9\. Beads / Persistent Memory

Gastown's "beads" pattern: immutable, timestamped records of every decision + outcome. Agents query past beads via SQL over a git-backed data plane. Turns every session into searchable institutional memory.

**Why:** Goes beyond static docs - agents learn from historical context.

Three pro-tips for smarter agents. Seven: force REFLECTION.md after every task - what surprised me, one AGENTS.md proposal, one prompt improvement. Lead reviews and merges learnings. Eight: token budgets per agent (frontend 180k, backend 280k), auto-pause at 85%, kill after 3+ stuck iterations. Nine: Gastown's beads pattern - immutable timestamped records of decisions queryable via RAG. Turns sessions into searchable institutional memory. ═══════ EXTENDED NOTES ═══════ Three more pro-tips on making agents smarter over time. Pro-tip seven: structured self-reflection. After every task completion, force the agent to output a REFLECTION.md with three sections: what surprised me, one pattern to propose for AGENTS.md, and one prompt improvement suggestion. The lead reviews these and merges approved learnings through the quality gate. This is how compound learning actually compounds - not ad hoc, but systematically after every single task. Pro-tip eight: token budgeting and kill criteria. This is the money saver. Set hard per-agent budgets - frontend teammate gets 180k tokens, backend gets 280k. When any agent hits eighty-five percent of its budget, it auto-pauses and notifies the lead. Also define kill criteria: if an agent is stuck for three-plus iterations on the same error, stop it and reassign the task to a fresh agent. Gastown and Conductor both expose budget controls now. Pro-tip nine: beads, which is Gastown's persistent memory pattern. Instead of just a static AGENTS.md file, beads are immutable, timestamped records of every decision and outcome. Agents can query past beads via RAG - retrieval augmented generation. This turns every session into searchable institutional memory that goes far beyond a flat markdown file.

PATTERN Ship While You Sleep

## The Ralph Loop - Self-Healing Continuous Agents

Co-popularized by Geoffrey Huntley & Ryan Carson (snarktank/ralph + Antfarm) · Atomic tasks → validate → commit → reset → repeat

THE LOOP CYCLE

1

**Pick** next task from tasks.json

2

**Implement** the change

3

**Validate** - tests, types, lint

4

**Commit** if pass, update status

5

**Reset** context & repeat

Stateless-but-iterative: fresh agent each run avoids context overflow. Small bounded tasks = fewer hallucinations.

SELF-HEALING SAFEGUARDS

✓Fail → feed error back → auto-retry

✓Stuck 3+ iterations → kill & reassign

●Feature branch only, sandbox execution

●Max iteration + time + token limits

→Multi-model cross-check, periodic fresh starts

→Agent opens PR — human reviews before merge

FOUR CHANNELS OF MEMORY

Git Commit History

Agent reads repo via git diff - no recall needed

Progress Log

Chronological journal: what was tried, pass/fail

Task State (tasks.json)

Persists which tasks are done, pending, or failing

AGENTS.md

Long-term semantic memory - wisdom compounds

SCALE

**Today:** 1 loop overnight  
**Next:** 10 loops, 10 branches

YOUR NEW ROLE

Curate the process.  
You're the EM for your AI team.

The Ralph Loop: break work into small atomic tasks, run an agent in a loop. Five steps: pick task, implement, validate, commit if pass, reset context and repeat. Stateless-but-iterative avoids context overflow. Key safeguards: auto-retry on failure, kill after 3+ stuck iterations, feature branches only, hard token/time limits. Agent opens PR, you review before merge. Four memory channels persist across resets: git history, progress log, tasks.json, and AGENTS.md. Start with one loop overnight, graduate to ten. ═══════ EXTENDED NOTES ═══════ If you saw Ryan Carson's talk earlier, this will look familiar - and that's intentional. The Ralph Loop, popularized by Geoffrey Huntley and refined by Ryan, is the pattern behind shipping while you sleep. Ryan's Antfarm tool for OpenClaw is one of the best implementations of this - it gives you a full agent team with planner, developer, verifier, tester, and reviewer roles, all running the Ralph loop pattern with deterministic workflows. What I want to do here is generalize the pattern so you can apply it regardless of which tool you're using - Claude Code, OpenClaw with Antfarm, Cursor, whatever your stack is. The core idea: break development into many small atomic tasks and run an AI agent in a loop. Each iteration follows a five-step cycle: pick the next task, implement it, validate with tests and type checks, commit if checks pass, then reset the agent context and repeat fresh. The key insight is stateless-but-iterative - by resetting each iteration, the agent avoids accumulating confusion. Small bounded tasks produce cleaner code with fewer hallucinations than one enormous prompt. But you need safeguards to make this reliable. Validation: run tests after every task, feed errors back for auto-retry, but kill and reassign if stuck for three-plus iterations. Risk management: always work on feature branches, sandbox execution, set hard limits on iterations, time, and tokens. And crucially, the agent opens a PR at the end - you review before merge. The loop maintains four channels of memory across iterations: git commit history, a progress log, the task state file, and AGENTS.md as long-term semantic memory. Each improvement makes future improvements easier. Start with one loop overnight, wake up to PRs. Then graduate to ten loops on ten branches.

THE HONEST TRUTH

## The human bottleneckwas a feature, not a bug.

When humans are slow, errors compound slowly and pain forces correction.  
Remove the bottleneck, and small mistakes compound at a rate  
that outruns your ability to catch them.

Human pace

Errors compound slowly.  
Pain is felt early.  
You fix as you go.

Agent pace

Errors compound fast.  
Pain is delayed.  
You notice too late.

The human bottleneck was a feature, not a bug. At human pace, errors compound slowly and pain forces early correction. With an army of agents, small mistakes - code smells, duplication, unnecessary abstractions - compound at unsustainable rates. You don't feel the pain until it's too late. This is why every quality gate matters: plan approval, hooks, token budgets, human review. Without them, you'll agentically code yourself into a corner. ═══════ EXTENDED NOTES ═══════ Here's the honest truth that anyone teaching agent orchestration needs to say out loud. The human bottleneck was a feature, not a bug. When humans write code slowly, errors compound slowly. You feel the pain early - a test fails, a code review catches something, you notice the duplication. And because pain is immediate, you fix as you go. With an orchestrated army of agents, there is no bottleneck. Small harmless mistakes - a code smell here, a duplication there, an unnecessary abstraction - compound at a rate that's unsustainable. You've removed yourself from the loop, so you don't feel the pain until it's too late. Then one day you try to add a feature, and the architecture, which is largely compounded mistakes at this point, doesn't allow it. Your tests are equally untrustworthy because agents wrote those too. This is why every quality gate we've discussed matters. This is why plan approval, hooks, token budgets, and human review exist. Not because they're nice to have - because without them, you will agentically code yourself into a corner.

DISCIPLINE

## Delegate the tasks.Not the judgment.

### Let agents do

Scoped tasks with clear pass/fail criteria  
Boilerplate, migrations, test scaffolding  
Exploring approaches you'd never try by hand  
Anything with a tight evaluation function

### Keep for yourself

Architecture and API design  
Deciding what *not* to build  
Reviewing agent output with full context  
The taste and friction that produce good systems

Build fewer features, but the right ones.  
The speed of code generation is a siren song.  
Slow down enough to maintain understanding.

Delegate tasks, not judgment. Agents excel at scoped tasks with clear pass/fail, boilerplate, migrations, test scaffolding. Keep for yourself: architecture, deciding what NOT to build, reviewing with full context, and the taste that produces good systems. Build fewer features but the right ones. Speed of code generation is a siren song. Slow down enough to maintain understanding - lose that and you lose the ability to fix or extend your system. ═══════ EXTENDED NOTES ═══════ So here's the discipline that makes all of this work. Delegate the tasks, not the judgment. On the left, what agents are genuinely great at: scoped tasks with clear pass or fail criteria, boilerplate, migrations, test scaffolding, exploring different approaches you'd never have time to try by hand. Anything where there's a tight evaluation function - the agent can measure its own work. On the right, what you should keep for yourself: architecture and API design, because agents have seen tons of bad architecture in their training data and will happily cargo-cult enterprise patterns into your startup. Deciding what NOT to build - saying no is a feature agents don't have. Reviewing agent output with full system context, because agents only ever have a local view. And the taste and friction that produce good systems. That friction - the slowness of having to think through a design yourself - is what lets your experience shape the system. The bottom line: build fewer features, but the right ones. The speed of code generation is a siren song. Slow down enough to maintain understanding. Because if you lose understanding of your own system, you've lost the ability to fix it, extend it, or even know when it's broken. All the orchestration patterns in this talk - quality gates, plan approval, hooks, AGENTS.md - exist to keep you in the loop. Not as a bottleneck, but as the judgment layer that makes everything else trustworthy.

LEVERAGE

## Your spec is the leverage.

When you orchestrate fifty agents in parallel, vague thinking  
doesn't just slow you down. It *multiplies*.

VAGUE SPEC

Ambiguous requirements propagate through dozens of parallel runs, each going slightly wrong in a slightly different direction.

PRECISE SPEC

Clear architecture, integration boundaries, edge cases, and invariants multiply into precise implementations across the entire fleet.

This is why strong engineers get **more** leverage from agents, not less.  
The mechanical work is automated. The cognitive work is amplified.

At fleet scale, vague specs don't just slow you down - they multiply errors across dozens of parallel runs. You need deep architectural understanding to write specs that survive autonomous execution. The spec is not a prompt anymore - it's product thinking made explicit. Strong engineers get MORE leverage from agents, not less. Mechanical work is automated; cognitive work is amplified across the entire fleet. ═══════ EXTENDED NOTES ═══════ Here's the insight that reshapes how you think about your own value. If you can orchestrate twenty, thirty, fifty agents running in parallel, the difference between mediocre output and exceptional output comes down almost entirely to the quality of your specification. At fleet scale, vague thinking doesn't just slow you down - it multiplies. Ambiguous requirements propagate through dozens of parallel autonomous runs, each one going slightly wrong in a slightly different direction. Poor architectural decisions made upfront don't affect one implementation - they propagate across the entire fleet. You can't write a spec that survives that environment unless you deeply understand the architecture, the integration boundaries, the edge cases, the failure modes, and the invariants that must never break. The spec is not a prompt anymore. The spec is the product thinking made explicit. This is why strong software engineers get more leverage from these tools than weak ones, not less. The mechanical work of typing code is being automated. The cognitive work of understanding systems is being amplified. Every hour you spend developing genuine architectural understanding now pays dividends across an entire fleet of autonomous workers rather than just your own output.

## BUILD THE FACTORY

You are no longer just writing code. You are building the factory that builds your software.

Plan

Write specs with  
acceptance criteria

→

Spawn

Create team,  
assign agents

→

Monitor

Watch progress,  
resolve blockers

→

Verify

Run tests,  
review code

→

Integrate

Merge branches,  
resolve conflicts

→

Retro

Update AGENTS.md,  
refine prompts

### PRACTICAL TIPS

• **Set WIP limits:** Don't run more agents than you can meaningfully review. 3-5 is the sweet spot.

• **Define kill criteria:** If an agent is stuck for 3+ iterations on the same error, stop and reassign.

• **Async check-ins:** Check progress every 5-10 minutes. Don't hover - let agents work autonomously.

• **One file, one owner:** Never let two agents edit the same file. Conflicts kill velocity.

You're building the factory that builds your software. Six-step production line: Plan (specs with acceptance criteria), Spawn (assign agents), Monitor (check every 5-10 min), Verify (tests and review - the real bottleneck), Integrate (merge branches), Retro (update AGENTS.md). Four tips: WIP limit 3-5 agents, kill after 3+ stuck iterations, async check-ins not hovering, one file one owner. ═══════ EXTENDED NOTES ═══════ Here's the factory model. You are no longer just writing code. You are building the factory that builds your software. That factory consists of fleets of agents. Each agent has a task, a toolbelt, context, and a feedback loop. A factory has quality control. A factory has process documentation. A factory has inputs that need to be precisely specified or the outputs come out wrong. A factory stalls when the environment is unreliable. All of these properties map directly onto agentic software development. Here's the six-step production line. Step one, Plan: write specs with clear acceptance criteria - your spec is the leverage, remember. Step two, Spawn: create your team and assign agents. Step three, Monitor: watch progress and resolve blockers every five to ten minutes. Step four, Verify: run tests, review code - verification is the bottleneck, not generation. Step five, Integrate: merge branches, resolve conflicts. Step six, Retro: update AGENTS.md with new patterns. Four practical tips. Set WIP limits - three to five agents is the sweet spot. Define kill criteria - stuck three-plus iterations, stop and reassign. Async check-ins - don't hover, let agents work. One file, one owner - conflicts kill velocity.

## 5 PATTERNS TO START TODAY

1

### Subagents for decomposition

Use the Task tool to spawn focused child agents. Give each one a specific brief and file ownership. Start here - zero setup needed.

2

### Agent Teams for parallelism

Enable CLAUDE\_CODE\_EXPERIMENTAL\_AGENT\_TEAMS=1. Create a lead + 3 teammates. Use shared task list for coordination.

3

### Git worktrees for isolation

Each agent gets its own worktree. No merge conflicts, clean integration. Tools like Conductor handle this automatically.

4

### Quality gates for trust

Require plan approval for risky changes. Add hooks that run tests on task completion. Never trust agent output without verification.

5

### AGENTS.md for compound learning

Document patterns, gotchas, and style preferences in AGENTS.md. Every session reads it, every session updates it. Knowledge compounds.

Five patterns to start today. One: subagents for decomposition - spawn focused child agents with specific briefs, zero setup. Two: Agent Teams for parallelism - shared task list, peer messaging, 3 teammates. Three: git worktrees for isolation - no merge conflicts. Four: quality gates for trust - plan approval, hooks, never trust without verification. Five: AGENTS.md for compound learning - every session reads it, every session updates it. Start with pattern one today, graduate to teams next week. ═══════ EXTENDED NOTES ═══════ If you take away five things from this talk, make it these five patterns. Number one: subagents for decomposition. Use the Task tool to spawn focused child agents. Give each one a specific brief and file ownership. This requires zero setup and you can start today. Number two: Agent Teams for parallelism. Enable the experimental flag, create a lead plus three teammates, and use the shared task list for coordination. Number three: git worktrees for isolation. Each agent gets its own worktree - no merge conflicts, clean integration. Tools like Conductor handle this automatically. Number four: quality gates for trust. Require plan approval for risky changes, add hooks that run tests on task completion, and never trust agent output without verification. Number five: AGENTS.md for compound learning. Document patterns, gotchas, and style preferences. Every session reads it, every session updates it. Knowledge compounds over time. Start with pattern one today, graduate to Agent Teams next week, then layer on quality gates and compound learning.

ADVANCED

## 10 Pro Moves for 10x Velocity

1

Hierarchical Subagents

Teams of teams - 3x decomposition depth

2

Loop Guardrails + Reflection

MAX\_ITERATIONS=8, forced self-correction

3

Dedicated @reviewer

1 reviewer per 3–4 builders, auto-triggered

4

Multi-Model Routing

Opus plans, Sonnet builds, security model reviews

5

Worktree Lifecycle Scripts

agent-spin / agent-merge / agent-clean

6

Human-Curated AGENTS.md

AI-written rules marginally reduce success rates

7

REFLECTION.md Proposals

Agents propose, leads approve learnings

8

Token Budgeting

Per-agent caps, 85% auto-pause

9

Beads / Persistent Memory

Immutable decision records, SQL-queryable

10

A2A Interoperability

Google A2A: Claude ↔ Copilot ↔ Codex handoff

These are the patterns senior engineers running 8–20 agents in production use *right now*.

Your cheat sheet - ten pro moves, rapid-fire. Hierarchical subagents for 3x decomposition. Loop guardrails with forced reflection. Dedicated @reviewer per 3-4 builders. Multi-model routing - Opus plans, Sonnet builds. Worktree lifecycle scripts. Human-curated AGENTS.md only. REFLECTION.md proposals. Token budgeting with auto-pause. Beads for persistent RAG-queryable memory. A2A protocol for cross-vendor handoff. These are what senior engineers running 8-20 agents use right now. Pin this slide. ═══════ EXTENDED NOTES ═══════ And here's your cheat sheet - ten pro moves for ten-x velocity. We covered each of these throughout the talk, so let me rapid-fire through them. One: hierarchical subagents - teams of teams, three-x decomposition depth. Two: loop guardrails plus reflection - MAX\_ITERATIONS equals eight with forced self-correction. Three: a dedicated reviewer teammate - one reviewer per three to four builders, auto-triggered on task completion. Four: multi-model routing - Opus plans, Sonnet builds, a security model reviews. Five: worktree lifecycle scripts - agent-spin, agent-merge, agent-clean. Six: human-curated AGENTS.md only - AI-written rules actually reduce success by three to twenty percent. Seven: REFLECTION.md proposals - agents propose learnings, leads approve them. Eight: token budgeting - per-agent caps with eighty-five percent auto-pause. Nine: beads for persistent memory - immutable decision records queryable via RAG. And ten: A2A interoperability - Google's protocol for Claude, Copilot, and Codex to hand off tasks to each other. These are the patterns senior engineers running eight to twenty agents in production are using right now. Pin this slide.

## RESOURCES

TOOLS MENTIONED IN THIS TALK

**Claude Code Teams**  
code.claude.com/docs/en/agent-teams

**Claude Code Web**  
claude.ai/code

**Conductor by Melty Labs**  
conductor.build (macOS · free · BYOK)

**Vibe Kanban (BloopAI)**  
vibekanban.com · github.com/BloopAI/vibe-kanban

**GitHub Copilot Coding Agent**  
github.com/features/copilot/agents

**Jules by Google**  
jules.google.com

**Codex Web by OpenAI**  
chatgpt.com/codex · openai.com/codex

**Google Antigravity**  
antigravity.google (free · all platforms)

**Cursor Cloud Agents + Glass**  
cursor.com/agents

**OpenClaw + Antfarm (Ryan Carson) · Claude Squad · Gastown (Yegge)**  
github.com/snarktank/antfarm · github.com/smtg-ai/claude-squad · github.com/steveyegge/gastown

All resources on screen. Key links: Claude Code Teams at code.claude.com, Claude Code Web at claude.ai/code, Conductor at conductor.build, Vibe Kanban at vibekanban.com, Copilot Agent at github.com/features/copilot/agents, Jules at jules.google.com, Codex Web at chatgpt.com/codex, Antigravity at antigravity.google. Tool selection cheat sheet on the right. All demos are in the linked repo. ═══════ EXTENDED NOTES ═══════ Here are all the resources. For tools: Claude Code Teams docs are at code.claude.com, Claude Code on the web is at claude.ai/code, Conductor at conductor.build, Vibe Kanban at vibekanban.com, GitHub Copilot Coding Agent at github.com/features/copilot/agents, Jules at jules.google.com, Codex Web at chatgpt.com/codex, Google Antigravity at antigravity.google - all free public preview - and Cursor Automations at cursor.com/automations. For articles: "Conductors to Orchestrators" is on O'Reilly Radar and addyosmani.com, the 2026 Agentic Coding Trends guide is at beyond.addy.ie slash 2026-trends. And on the right, a quick tool selection cheat sheet so the audience knows exactly which tool to try first based on their situation. All five runnable demos are in the linked repository.

## THANK YOU

Addy Osmani

addyosmani.com • @addyosmani

The high-leverage developer of 2026 is an async-first manager  
running parallel AI agents.

The one thing to remember: the high-leverage developer of 2026 is an async-first manager running parallel AI agents. Start with subagents today, build up to Agent Teams, add quality gates, let compound learning do the rest. Find me at addyosmani.com or @addyosmani. I'd love to hear how you put these patterns into practice. Thank you. ═══════ EXTENDED NOTES ═══════ Thank you so much. The one thing I want you to remember: the high-leverage developer of 2026 is an async-first manager running parallel AI agents. The developers who will be most productive aren't the ones who write the most code - they're the ones who orchestrate the best outcomes from teams of AI agents. Start with subagents today, build up to Agent Teams, add quality gates, and let compound learning do the rest. I'm Addy Osmani - you can find me at addyosmani.com or on social at addyosmani. I'd love to hear how you put these patterns into practice. Thank you.