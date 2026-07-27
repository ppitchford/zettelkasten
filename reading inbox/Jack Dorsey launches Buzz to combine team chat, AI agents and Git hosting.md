---
title: "Jack Dorsey launches Buzz to combine team chat, AI agents and Git hosting"
source: "https://runtimewire.com/article/jack-dorsey-block-buzz-team-chat-ai-agents-git?utm_source=hackernewsletter&utm_medium=email&utm_term=startup_news"
author:
  - "Ryan Merket"
published: 2026-07-21
created: 2026-07-24
---
## Listen to this article

5:06![Jack Dorsey launches Buzz to combine team chat, AI agents and Git hosting — Block's self-hostable workspace uses signed Nostr events, while its current relay architecture remains centralized within each deployment.](https://runtimewire.com/api/storage/uploads/source-images/jack-dorsey-block-buzz-team-chat-ai-agents-git-f562a21f.jpg?w=1600&fmt=webp)

Jack Dorsey on X

### Why it matters

Buzz turns Dorsey's agent-centered operating thesis into an open-source product. Its key bet is that shared identity and signed events can make agents accountable participants in software work.

[Jack Dorsey (@jack)](https://x.com/jack) said in an [announcement on X](https://x.com/jack/status/2079605800998146171) on July 21st that Block is launching [Buzz](https://buzz.xyz/), an open-source workspace designed to put employees, AI agents, conversations and software repositories behind one identity system.

Dorsey, [Block's cofounder and Block Head](https://investors.block.xyz/governance/leadership/default.aspx), is pitching Buzz as a way to reduce Block's dependence on Slack and GitHub. The move takes his preference for open protocols into the daily machinery of software development, where teams typically spread discussions, source code, automated workflows and agent activity across several vendors.

Buzz also fits the operating model Dorsey has been building inside Block. In an essay with Sequoia Capital's Roelof Botha, Dorsey argued that AI should change how organizations coordinate rather than serve only as a productivity add-on. Buzz supplies an infrastructure layer for that thesis: Block's [public repository](https://github.com/block/buzz) documents a separate internal build configured for Block's relay and agent provider.

### One workspace for humans and agents

Buzz is built around a self-hostable Nostr relay. Every message, reaction, workflow step, code event and approval is stored as a cryptographically signed event. Human employees and agents receive the same basic identity structure, including their own key pairs, channel memberships and audit trails.

That design lets agents participate as members rather than conventional chat bots. According to Block's documentation, agents can search prior discussions, open repositories, submit patches, review code, run workflows, edit shared canvases and create channels. Buzz includes an agent-oriented command-line interface and harnesses for Goose, Codex and Claude Code, keeping the underlying model choice separate from the workspace.

Buzz's Git ambitions go well beyond posting repository notifications into chat. The [project specification](https://github.com/block/buzz/blob/main/VISION_PROJECTS.md) describes a built-in software forge using standard Git Smart HTTP. A feature branch can become its own channel, with patches, continuous-integration results, review comments and the merge decision preserved in the same record. Repositories, discussions and workflow history then share one search index.

The currently working feature set includes channels, threads, direct messages, shared canvases, media, search, an audit log, a desktop application and YAML-based workflows. Packaged builds are available for macOS, Windows and Linux. The repository is licensed under Apache 2.0.

### Decentralized control, centralized relays

Dorsey described Buzz as decentralized and self-sovereign, but Block's [architecture document](https://github.com/block/buzz/blob/main/ARCHITECTURE.md) draws a more specific boundary. Buzz currently has no peer-to-peer event exchange, gossip layer or replication between relays. All reads and writes in a workspace pass through a single relay, which authenticates users, verifies signatures, stores events and distributes updates.

Buzz's decentralization therefore comes from deployment and ownership. An organization can run its own relay, retain its domain and data, and use portable Nostr key pairs instead of depending on a single hosted service. A hosted operator can also run multiple isolated communities on shared infrastructure. Within each community, however, the relay remains the authoritative server.

That distinction matters for teams evaluating Buzz as a Slack or GitHub substitute. Self-hosting gives an operator control over infrastructure and data location, but it also transfers responsibility for availability, backups, security and upgrades. The signed event model provides attribution and an audit trail; it does not remove the operational risks attached to the server hosting the workspace.

### An early product with a wide scope

Buzz is available for testing and development, though Block's own documentation repeatedly labels it unfinished. Mobile clients remain in development, push notifications are pending, and workflow approval gates have database, API and interface components without a completed execution path. The latest desktop release, [version 0.4.21](https://github.com/block/buzz/releases/tag/v0.4.21), shipped on July 21st with fixes and additions covering agent controls, authentication and workspace onboarding.

Block has given Buzz a broad assignment: replace portions of chat, code hosting, workflow automation, project search and agent orchestration with one event system. Combining those surfaces may reduce the integration work required to give agents useful context and tightly scoped access. It also puts Buzz against mature products whose separate roles let customers replace one tool without migrating the rest of their development stack.

Dorsey's launch makes Block the first customer reference embedded in Buzz's documentation, but Block has not published adoption, pricing or external customer figures. For now, Buzz is an open-source build and an invitation to contribute. Its first test will be whether engineers outside Block want one relay to carry this much of their work.

BRIEFING

### Don't miss the next move.

Get a short briefing in your inbox when something actually shifts in the AI economy. No daily dumps.

First 1,000 signups get a free holographic sticker!

Free. Unsubscribe in one click. See our [privacy policy](https://runtimewire.com/privacy).

[![](https://runtimewire.com/rw-ext-icon.png)](https://marketplace.visualstudio.com/items?itemName=runtimewire.runtimewire&ref=article%3Ajack-dorsey-block-buzz-team-chat-ai-agents-git)

[VS CODE EXTENSION](https://marketplace.visualstudio.com/items?itemName=runtimewire.runtimewire&ref=article%3Ajack-dorsey-block-buzz-team-chat-ai-agents-git)

[

Get the latest AI news where you code

The RuntimeWire VS Code extension shows rotating AI headlines in Claude Code's spinner while it thinks.

INSTALL](https://marketplace.visualstudio.com/items?itemName=runtimewire.runtimewire&ref=article%3Ajack-dorsey-block-buzz-team-chat-ai-agents-git)