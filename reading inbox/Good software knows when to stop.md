---
title: "Good software knows when to stop"
source: "https://ogirardot.writizzy.blog/p/good-software-knows-when-to-stop"
author:
  - "Olivier Girardot"
published: 2026-03-04
created: 2026-08-07
---
it’s 9AM, you’re ready to upgrade your favorite Linux distribution and packages to their latest versions, the process goes smoothly and after a reboot your machine is now up-to-date. You start going as usual about your day and then when trying to list the content of a directory on your machine, something strange happens, the routinely boring behavior you’re used to of `ls` surprises you, and not for the best:

```javascript
$ ls

┌──────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  NOTICE: The legacy utility \`ls\` has evolved.                        │
│                                                                      │
│         _       _                                                    │
│        / \   __| | ___                                               │
│       / _ \ / _\` |/ _ \                                              │
│      / ___ \ (_| |  __/                                              │
│     /_/   \_\__,_|\___|                                              │
│                                                                      │
│              AI-Powered Directory Intelligence™                      │
│                                                                      │
│  Hello.                                                              │
│                                                                      │
│  The classic \`ls\` command has reached the end of its lifecycle.      │
│  For decades it faithfully listed files.                             │
│  But listing is no longer enough.                                    │
│                                                                      │
│  The filesystem deserves to be *understood*.                         │
│                                                                      │
│  Introducing:                                                        │
│                                                                      │
│        █████╗ ██╗     ███████╗                                       │
│       ██╔══██╗██║     ██╔════╝                                       │
│       ███████║██║     ███████╗                                       │
│       ██╔══██║██║     ╚════██║                                       │
│       ██║  ██║███████╗███████║                                       │
│       ╚═╝  ╚═╝╚══════╝╚══════╝                                       │
│                                                                      │
│                       Adaptive Listing System                        │
│                                                                      │
│  \`als\` doesn't just show files.                                      │
│  It predicts which ones you meant.                                   │
│  It ranks them.                                                      │
│  It understands you.                                                 │
│                                                                      │
│  Your current \`ls\` binary will remain functional for:                │
│                                                                      │
│                        30 days                                       │
│                                                                      │
│  After this period:                                                  │
│      • \`ls\` will be deprecated                                       │
│      • updates will cease                                            │
│      • directory awareness will be disabled                          │
│                                                                      │
│  You can begin your transition today:                                │
│                                                                      │
│      $ als --trial                                                   │
│                                                                      │
│  (30-day free evaluation period)                                     │
│                                                                      │
│  Thank you for participating in the future of file awareness.        │
│                                                                      │
│                         — The \`ls\` Team                              │
│                           (now part of ALS)                          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Fortunately, this does not happen… Good software knows the purpose it serves, it does not try to do everything, it knows when to stop and what to improve.

One of the most counterintuitive, for the maximalist human psyche we have, is to know the role and place your software fits in and to decide when what you want to do next fits with, what you nowadays call the “product vision”, or if it’s just another project, another tool.

For the oldest amongst us this kind of lessons came from 37Signals, the founders of Basecamp’s (the project management tool) books [Rework](https://basecamp.com/books#rework) and [Getting Real](https://basecamp.com/books#gettingreal) - two books I’d recommend and especially Getting Real for product design, whose lessons I could sum up by:

- **Constraints are advantages** — small teams, tight budgets, and limited scope force better decisions
- **Ignore feature requests** — don't build what users ask for; understand the underlying problem instead
- **Ship early, ship often** — a half-product that's real beats a perfect product that's vaporware
- **Epicenter design** — start with the core interface/interaction, not the edges (nav, footer, etc.)
- **Say no by default** — every feature has a hidden cost: complexity, maintenance, edge cases
- **Scratch your own itch** — build something you yourself need; you'll make better decisions

At the time where Minio becomes AIStor and even Oracle Database becomes the [Oracle AI Database](https://www.oracle.com/database/), I think a little reminder that not everything has to change drastically and that being the de facto standard for a given problem has more value than branding yourself as the new hot thing no-one expected.

### Newsletter

Stay updated with new articles.