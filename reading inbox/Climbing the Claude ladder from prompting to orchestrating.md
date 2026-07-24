---
title: "Climbing the Claude ladder: from prompting to orchestrating"
source: "https://www.instapaper.com/read/2000214564"
author:
  - "[[Garrett]]"
  - "[[31 Mar 2026]]"
published: 11 min
created: 2026-07-24
---
[betterthangood.xyz](https://betterthangood.xyz/blog/prompting-to-orchestrating/)

Most people using Claude are stuck on the first rung of a very tall ladder. They open a chat, type a question, get an answer, and move on with their day. Which is fine, but it’s a bit like buying a full workshop and only using the tape measure.

I’ve spent the better part of a year climbing this ladder at [Better Than Good](https://betterthangood.xyz/), and the difference between the bottom and the top is not incremental. It’s the difference between asking for directions and building the road.

Here’s how the progression works, what changes at each level, and why most people plateau long before they should.

## Prompting

Everyone starts here, and it looks the same every time. You open [claude.ai](https://claude.ai/), type something, and Claude responds. Maybe you’ve set up a Project with custom instructions so Claude already knows how you like things done. Either way, the interaction follows the same arc and ends the same way. You talk, Claude talks, the conversation ends, and everything you built together disappears.

The craft of prompting is real and worth learning, and [Anthropic’s own guidance](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) comes down to a simple idea. Treat Claude like a brilliant new hire who has zero context about your business, your codebase, or your preferences. Everything you want Claude to know, you have to say explicitly, every single time.

Certain techniques make a measurable difference at this level. [Wrapping distinct content in XML tags](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags) (`<instructions>`, `<context>`, `<example>`) gives Claude clear boundaries between what to do and what to work with. [Putting your reference documents at the top and your actual question at the bottom](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/long-context-tips) improves response quality by up to 30% on complex inputs. Giving [three to five diverse examples](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/multishot-prompting) of the output you want steers Claude’s format, tone, and depth far more reliably than describing what you want in prose.

These are good skills to have, and for a while, they feel like enough.

### Where prompting falls apart

The problem isn’t the quality of the output but the lifecycle of the input.

You spend 20 minutes crafting a prompt that gets Claude to produce exactly the kind of client proposal you need. The language is right, the structure is right, the tone matches your brand. You use it, it works, and you close the tab.

Two weeks later you need another proposal and you vaguely remember what you did, so you open a new chat and try to recreate it. You get close but not quite the same quality you had before. You tweak, you iterate, you burn another 20 minutes arriving at something you already figured out.

This is the same problem I wrote about in [Zero busy work](https://betterthangood.xyz/blog/zero-busy-work/). You’re doing configuration work over and over, manually rebuilding context that should already exist somewhere. It’s the AI version of answering the same question from your team for the third time instead of writing the documentation that makes the question go away. Every session is a cold start, and your best prompts live in your head as tribal knowledge that doesn’t compound.

The ceiling here isn’t Claude’s ability but the fact that nothing you learn together persists beyond the conversation, and that’s what pushes you toward the next rung.

## Skills

A skill is what happens when a prompt learns to survive between sessions.

In [Claude Code](https://code.claude.com/docs/en/skills), a skill is a directory containing a markdown file with YAML frontmatter and plain instructions. It lives on disk, version-controlled alongside your project, and Claude reads it at the start of a session and follows it whenever the context fits. You can invoke one explicitly with a slash command (`/deploy`, `/review`, `/publish`) or Claude can trigger it automatically based on the description you wrote in the frontmatter.

That’s the mechanical explanation, and the practical one is more interesting.

### The anatomy of a skill

A skill file has two parts, and the split is clean. The frontmatter tells Claude when and how to use it, and the body contains the actual instructions.

```js
---
name: write-proposal
description: Draft a client proposal from intake notes
---

Read the proposal template at \`templates/proposal.md\` and the client's intake notes. Use the examples in \`examples/past-proposals/\` to match our tone, structure, and level of detail.

## Steps

1. Ask which client this proposal is for
2. Read their intake notes from the client folder
3. Fill in the proposal template with the client's details, goals, and pricing
4. Save the draft to the client's folder for review.
```

Unlike a chat prompt, a skill can reference supporting files. Templates for Claude to fill in, example outputs to match, and dynamic context pulled in at load time. A single skill can carry an entire workflow in a way that a conversation never could.

The [CLAUDE.md system](https://code.claude.com/docs/en/memory) works alongside skills as the persistent memory layer. It’s a markdown file loaded every session that tells Claude about your project’s conventions, architecture preferences, and working agreements. Path-specific rules activate only when Claude touches matching files, and auto-memory accumulates notes across sessions so that everything compounds.

We use skills extensively at BTG and our project onboarding skill is a good example. It orchestrates five sections of setup work (Slack channels, Google Drive folders, Harvest billing, project configuration, and timeline creation) with each section carrying its own markdown file, shared conventions, and config references. You don’t rebuild that from a chat prompt (or worse, 2023 it) every time a new project kicks off.

### The ceiling

Skills make Claude dramatically better at individual tasks. The quality goes up, the consistency goes up, and the ramp-up time for each session drops to nearly zero. If prompting is a 1x baseline, skills get you to a comfortable 2-3x on quality and consistency.

But you’re still working with one Claude instance, one conversation, one task at a time. The skill makes the agent better at that task, but the throughput ceiling is still one and you can only move as fast as a single context window allows.

Breaking through that ceiling requires agents working in parallel.

## Agent orchestration

This is where the math changes entirely.

Agent orchestration means coordinating multiple Claude instances, each with their own context window, tools, and specialization, working in parallel on decomposed pieces of a larger task.

Instead of one Claude doing everything sequentially, you have a team. A lead agent breaks down the work, assigns tasks with clear ownership, and specialist agents execute in parallel. Each specialist carries only the context it needs, which means its context window stays clean and focused instead of bloated with everything.

[Addy Osmani](https://addyosmani.com/blog/code-agent-orchestra/) put it well when he observed that three focused agents consistently beat one generalist agent working three times as long. It’s the classic jack of all trades, master of none problem applied to AI. A single agent trying to research, strategize, and critique in one session becomes mediocre at all three. The gains from splitting them up come from parallelism (they work simultaneously), isolation (no context pollution between tasks), and specialization (each agent is configured for its domain).

### How it works in practice

Claude Code already supports this at multiple levels in production today. Subagents are specialized instances that run in isolated context windows with custom system prompts and tool restrictions, defined in markdown files just like skills, and the parent agent spawns them as needed.

[Agent Teams](https://code.claude.com/docs/en/agent-teams) take this further with a team lead coordinating two to five teammates, each an independent Claude Code instance with its own million-token context. They share a task list with dependency tracking, send peer-to-peer messages (they DM eachother!), and claim work from a queue. File locking prevents conflicts, and when a dependency resolves, blocked tasks automatically unblock and pick up where they left off.

The `/batch` skill that ships with Claude Code is the canonical example of all of this in action. It researches a codebase, decomposes a change into five to thirty units, spawns one agent per unit in an isolated git worktree, has each agent run tests, and opens a pull request for each one. The human reviews and merges, but the production line runs itself.

That’s a code-heavy example, so here’s one that isn’t.

### A researcher, a strategist, and a devil’s advocate walk into a bar

Say you need a go-to-market strategy for a new service offering. In a prompting world, you’d sit with Claude and work through the research, the strategy, and the critique in one long conversation. By the time you get to the critique, Claude’s context is stuffed with research notes and half-formed strategy drafts, and the quality of the pushback suffers for it.

With orchestration, you tell your orchestrator what you need and it creates the team required to do it.

The **researcher** goes first with access to web search, your internal docs, and a skill that knows how to structure market research. It gathers competitive intelligence, identifies trends, and then does something interesting. It spins up persona subagents, synthetic versions of your ideal customers, and interviews them using questions it generated from the research. The interview transcripts and research summary get packaged up and handed off.

The **strategist** picks up that package with a clean context window. It has access to a different set of skills, ones that know your positioning framework, your pricing model, and your brand voice. It takes the raw research and builds a strategy document from it, working from a template that encodes your preferred structure and depth.

The **devil’s advocate** receives the finished strategy and nothing else, no research notes, no early drafts. Its only job is to pressure-test and break the thing apart. It has a skill that knows how to pressure-test assumptions, find logical gaps, and flag risks. Its critique goes back to the strategist, who revises the document with the feedback incorporated and delivers a final version for your review.

Each agent has different rules, different tools, different memory, and a context window that only contains what it needs. The researcher doesn’t know about your pricing model and the devil’s advocate hasn’t seen the raw research. The strategist gets the benefit of both without the noise of either.

And we’re not even to inception yet, because each of those agents could be managing subagents of their own.

### Your brief is the multiplier

Here’s the part that surprises people when they get to this level. Agent orchestration doesn’t reduce the need for clear thinking, it amplifies the consequences of unclear thinking dramatically.

A vague brief sent to one agent produces one mediocre result. The same vague brief sent to five parallel agents produces five mediocre results faster, and you’ve multiplied the mess instead of the output. Tell your research agent to “look into the competition” and you’ll get a generic summary that helps no one. Tell it to identify the top three competitors in your region, compare their onboarding process to yours, and interview five synthetic personas about what would make them switch, and you get something you can act on. The quality of your upfront thinking determines whether parallelism is a multiplier or a chaos engine, which is why the people who know their business best get the most out of agents.

Even with a strong brief, five agents producing work simultaneously means five streams of output landing on your desk at once. The bottleneck shifts from producing work to reviewing it, and the quality gates matter more at this tier, not less. You need approval checkpoints before agents move between phases and sometimes a dedicated reviewer agent whose only job is to poke holes in what the others produced. That human bottleneck was a feature, not a bug.

When orchestration is working well on both ends (clear briefs in, solid review out), you’re looking at 5-10x or more of throughput compared to working with a single agent. Not because each agent is faster, but because five of them are running at once and each one is focused on what it does best.

## Orchestrating orchestrators

And then there’s the level above all of that.

Rather than spawning six agents yourself and managing all of them, you spawn two leads who each spawn and manage two to three specialists. You’re managing two direct reports and they’re managing theirs. Your context stays clean because you’re only tracking high-level progress, not every file change or decision made.

This is hierarchical delegation, and it maps to how organizations already work. A CEO doesn’t manage every engineer, they manage a few leaders who manage teams who have engineers.

The tooling at this level ranges from agents running on your own machine, to local orchestrators managing three to ten agents with shared task lists, to cloud-based systems where you hand off work and it runs on someone else’s infrastructure entirely. You pick the tier that fits the task.

The repeatable pattern looks the same regardless. You provide high-level intent (“we need a go-to-market plan for this new service” or “onboard this new client”). An orchestrator breaks that into tasks and figures out the order. Specialist agents work in parallel, each in their own workspace. Quality checkpoints enforce your standards before anything moves forward, and the results come back to you for final review.

Cost management becomes very real at this level, too. You set budgets per agent, route different types of work to different models (the cheap ones handle planning, the powerful ones handle the heavy lifting), and build in rules so that an agent stuck on the same problem three times in a row gets reassigned instead of burning through your budget.

The compound learning is what makes this fly over time. Every session reads the memory files, and approved updates from previous sessions compound the knowledge base so that the first time you run a workflow it discovers what works and what doesn’t, and the second time it avoids the mistakes entirely. The system gets smarter without anyone sitting down to rewrite it.

## The ladder is really about compounding

Looking at this from the top, the pattern is unmistakable.

At each tier, the work you did at the previous tier doesn’t disappear but becomes the raw material for the next one. Good prompts become skills, good skills become agent instructions, and good agent patterns become orchestration playbooks.

The businesses and individuals who are getting the most out of these tools are the ones who climb the ladder deliberately, building each rung before reaching for the next one. It’s not about having the fanciest setup but about having a setup where every session makes the next one better.

That compounding effect is what we’re constantly researching and building at Better Than Good. Not just using Claude, but building the systems that make Claude more useful every time we sit down to work.