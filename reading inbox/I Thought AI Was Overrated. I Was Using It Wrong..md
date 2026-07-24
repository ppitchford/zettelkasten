---
title: "I Thought AI Was Overrated. I Was Using It Wrong."
source: "https://www.instapaper.com/read/1997503216"
author:
  - "[[Avinash Shekar]]"
published: 3 min
created: 2026-07-24
---
Most people quit AI too early.

They try it.  
Get a half-right answer.  
Watch it hallucinate something absurd.

Then conclude:

> *“AI isn’t there yet.”*

I thought that too.  
I was wrong.  
The problem wasn’t intelligence.  
It was infrastructure.

Press enter or click to view image in full size

## The Real Issue

You’re asking AI to reason about your:

- Architecture
- Conventions
- Build system
- Edge cases

Without giving it any of that context.  
It’s like hiring a contractor and hiding the blueprints.  
Of course it’s inconsistent.

Press enter or click to view image in full size

The shift for me was simple:  
Stop treating AI like a chatbot.  
Start treating it like engineering infrastructure.

Everything changed.

## The 6 Layers That Made AI Reliable

Each layer fixes one failure mode.  
Stack them together, and AI output stops feeling random.

## 1\. Context Files (Stop Making It Guess)

First breakthrough:

Press enter or click to view image in full size

Tell the AI what it’s working with.

Generate structured context files that describe:

- Stack
- Directory structure
- Commands
- Conventions

Now it doesn’t guess your package manager.  
It doesn’t invent frameworks.  
It doesn’t assume the wrong tooling.

The error rate drops immediately.  
Checkout repo — [AgentSeed](https://github.com/avinshe/agentseed) for generating context files.

## 2\. Architecture Memory (Teach It Why)

Context tells AI what your project is.  
Architecture memory tells it why decisions were made.

Press enter or click to view image in full size

These are mistakes that once cost hours.  
Without memory, AI rediscovers them.

With memory, it avoids them.

## 3\. Plan Before Code (Massive ROI)

Most people let AI start coding immediately.  
That’s the trap.

Press enter or click to view image in full size

Force plan mode.

The AI must:

- Explore the repo
- Identify affected files
- Propose an approach

Only after review does it write code.

## Get Avinash Shekar’s stories in your inbox

Join Medium for free to get updates from this writer.

A bad plan caught early costs minutes.  
A bad implementation costs hours.  
Planning compresses risk.

## 4\. Live Validation (Kill Hallucinations)

Some errors only disappear when confronted with reality.

API responses.  
Database schemas.  
Production data.

So connect AI to live systems. With limited read-only attempts, of course!

Press enter or click to view image in full size

If it expects:

```js
{ data: [] }
```

But reality returns:

```js
{ results: [], meta: {} }
```

It corrects itself instantly.  
Hallucinations don’t survive verification.

## 5\. Skills (Process > Prompts)

Every feature follows the same pattern:

1. Plan
2. Implement
3. Validate
4. Run tests
5. Lint
6. Commit

So encode it as a reusable workflow.

Press enter or click to view image in full size

Now AI follows the process automatically.

No skipped steps.  
No forgotten validation.

This is when AI stops feeling like autocomplete  
and starts feeling like a senior engineer with discipline.

## 6\. Tests as Contracts

Final layer.

Press enter or click to view image in full size

Write tests.  
Let AI implement against them.

The tests are the specification.  
There’s no “looks good.”

There’s only:  
Pass.  
Or fail.

Binary truth eliminates ambiguity.

## Why Most People Give Up on AI

Because they never build a system around it.

They try five prompts.  
Get inconsistent output.  
Quit.

Raw AI *is* unreliable.

But infrastructure makes it predictable.

Each layer removes a failure mode:

- Context → wrong assumptions
- Memory → repeated mistakes
- Planning → premature coding
- Validation → hallucinated data
- Skills → inconsistent workflow
- Tests → unclear success criteria

Individually simple.  
Collectively transformative.

**The next generation won’t debate whether AI works.  
They’ll debate who built the better system around it.**