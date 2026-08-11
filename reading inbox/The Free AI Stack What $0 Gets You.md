---
title: "The Free AI Stack: What $0 Gets You"
source: "https://sumguy.com/free-ai-stack-what-zero-gets-you/"
author:
  - "SumGuy"
published: 2026-08-09
created: 2026-08-11
---
## Everyone Wants To Sell You A Subscription. Ignore Most Of Them.

Every AI company wants you to believe you need their $20 a month plan to get anything real done. You don’t. Stitch together the free tier from five or six different providers and you end up with a chat assistant, a code helper, an image generator, and speech tools that would have cost you a small SaaS budget two years ago. The trick isn’t finding one free provider who does everything, because nobody does that well. The trick is figuring out which provider gives away which capability for free, and routing each job to whoever’s footing the bill for your tokens today.

This isn’t a “10 best free AI tools” listicle. It’s an inventory, organized by what you’re actually trying to do: talk to a model in a browser, call an API from a script, generate an image, transcribe audio, get code written. For each bucket I’ll tell you who’s genuinely free (not “free trial,” not “free for your first 14 days”), what the real limit is, whether they want a credit card up front, and what the tier is good for versus where it leaves you stranded.

Limits verified 2026-08-09. Free tiers move fast. Google cut Gemini API quotas by 50 to 80 percent in December 2025 with no warning, and OpenRouter’s free model roster has already rotated twice this year. Treat every number below as a snapshot, not a promise, and check the provider’s current page before you build something that depends on one staying put.

## Hosted Chat UIs: The Free Buffet

If all you need is a browser tab and a place to paste text, this is the easiest bucket on the list. No key, no billing page, no rate-limit headers to parse. Just an account, and in most cases not even that.

The catch is that “free” chat has quietly become “free, until the model decides you’ve used enough compute.” The old model of counting messages is mostly gone. What’s left is a compute-weighted pool that refills on a timer, which means your actual daily budget depends on how long your prompts are and how hard the model has to think.

| Provider | Real limit (2026-08-09) | Card required | Best for |
| --- | --- | --- | --- |
| ChatGPT Free | Roughly 10 messages on the flagship model every 5 hours, then it drops you to a smaller model | No | Quick one-off questions, not extended back and forth |
| Claude.ai Free | Session and weekly caps that scale with demand, commonly a dozen or so messages before you hit a wall | No | Writing and reasoning in short, focused bursts |
| Gemini Free | Compute-based allowance refreshing every 5 hours, with a weekly ceiling on top | No | Long-context tasks and anyone already living in Google Workspace |
| DeepSeek Chat | No published per-query cap, runs the current DeepSeek V4 with an Expert and an Instant mode | No | The closest thing to an unlimited daily driver in this bucket |
| Together Chat | Roughly 10 credits a day for DeepSeek R1, about 50 credits a day for the rest of its open-model lineup | No | Comparing open models side by side without installing anything |

DeepSeek’s web chat is the one worth bookmarking if you’re going to lean on a browser tab all day. It doesn’t publish a hard ceiling the way the US labs do, and in practice you can hammer it far harder before it pushes back. The downside is exactly what you’d expect from a free consumer product: your prompts are training data, so don’t paste anything you wouldn’t want summarized back to you by a stranger someday.

## Free API Quota: When You Actually Need A Key

This is the bucket that separates “I want to ask an AI a question” from “I want to build something.” A key means rate limits you can hit programmatically, quotas measured in requests per minute instead of vibes, and the option to wire a model into a script, a bot, or a cron job.

| Provider | Free tier (2026-08-09) | Card required | Best for |
| --- | --- | --- | --- |
| Google AI Studio (Gemini API) | Per-project RPM and RPD caps that vary by model, Flash-Lite is the most generous, Pro the stingiest, and December 2025 quota cuts hit every tier hard | No | Prototyping against Gemini before you commit to paying for it |
| OpenRouter | 20 requests per minute on any `:free` model, 50 requests a day baseline, jumping to 1,000 a day for life after a single one-time $10 top-up | Only for the optional $10 unlock | Testing a rotating cast of open models through one API key |
| Groq | Org-level caps: tens of requests per minute, thousands of tokens per minute, a daily request ceiling in the thousands, all varying by model | No | Fast inference when latency matters more than raw volume |
| Cerebras | 1 million tokens a day, single-digit requests per minute, limited to two models | No | Watching a response stream faster than you can read it |
| Mistral La Plateforme | An “Experiment” tier with roughly a billion tokens a month across the full lineup, including Codestral | No | Trying Mistral’s code models before you decide to pay |

OpenRouter deserves its own callout because the mechanics are unusual: that $10 top-up isn’t a subscription and it isn’t a balance you burn through, it’s a one-time unlock that permanently raises your free-model ceiling from 50 requests a day to 1,000. Pay ten bucks exactly once and you’ve bought yourself a meaningfully bigger free tier forever. The catch is the model roster itself rotates constantly. Qwen3-Coder’s free endpoint on OpenRouter was gone by early August after being everyone’s top pick in June, so whatever list you find today is a snapshot, not a lease.

Cerebras is the odd one out and worth calling out separately: it’s not trying to compete on quota, it’s trying to make you feel the speed of wafer-scale inference chips. A million tokens a day sounds huge until you notice the requests-per-minute cap means you’re not going to build a chatbot on it. Use it for the “holy hell that’s fast” demo, not for production traffic.

## Image Generation: Pick Your Watermark Poison

Every free image tier ships with some kind of asterisk. Sometimes it’s a visible watermark, sometimes it’s an invisible one baked into the pixels (SynthID, C2PA metadata), sometimes it’s a hard daily cap that resets at a time zone you didn’t choose. None of that makes them useless, it just means you need to know which flavor of compromise you’re signing up for.

| Provider | Free tier (2026-08-09) | Card required | Best for |
| --- | --- | --- | --- |
| Google Gemini app (Nano Banana 2 / Imagen) | About 20 images a day at up to 1K resolution, invisible SynthID watermark, no visible mark | No | The best free image quality you’ll get, one image at a time in a browser |
| Bing Image Creator | About 15 fast generations a day, DALL-E 3 quality, invisible C2PA metadata only | No (Microsoft account) | Same idea as Gemini, if you’d rather use a Microsoft account |
| Pollinations.ai | No published cap, fair-use rate limited, no signup, no key at all | No | The only one of the three you can actually call from a script or a cron job |

Here’s the practical split: Gemini and Bing are for a human sitting at a keyboard who wants one polished image and is willing to click a button and wait. Pollinations is for automation, because it’s a URL you can construct and hit from code with zero authentication. If you’re generating a featured image for a blog post by hand once a day, use Gemini. If you’re generating fifty thumbnails in a batch job, Pollinations is the only one built for that job, and it’ll happily let you hammer it in a way that would get you rate-limited off the other two in about ninety seconds.

## Speech To Text And Text To Speech: The Lopsided Bucket

This is the one bucket on this list that isn’t a fair fight. Speech-to-text has a genuinely great free option. Text-to-speech doesn’t, and pretending otherwise would be lying to you.

| Provider | Free tier (2026-08-09) | Card required | Best for |
| --- | --- | --- | --- |
| Groq (Whisper Large V3 / Turbo) | 2,000 requests a day, about 2 hours of audio processed per clock hour, 50+ languages, OpenAI-compatible endpoint | No | Real transcription work, this is the best free deal in the entire stack |
| ElevenLabs | 10,000 characters a month (about 10 minutes of audio), 2,500 characters per request, no commercial usage rights | No | Testing what a voice sounds like, not shipping anything with it |
| Self-hosted Piper or Coqui | Unlimited, bounded only by your own hardware | N/A | The actual answer if you need TTS in production and your budget is $0 |

Groq’s Whisper endpoint is not a compromise pick. It’s free, fast, and covers more languages than you’ll ever need, and 2,000 requests a day is enough to transcribe a busy podcast backlog without breaking a sweat. Use it, don’t overthink it.

Text-to-speech is where the free tier story falls apart. ElevenLabs gives you ten minutes of audio a month and won’t let you use the output commercially even if you had a use for it, which makes the free tier a demo, not a tool. If you actually need TTS for something real (accessibility features, a home automation announcer, whatever), the honest free answer is to self-host Piper. It’s not going to sound like ElevenLabs’ best voices, but it’s yours, it’s unlimited, and it doesn’t ask you to justify your character count to anyone.

## Code Assistance And Agentic Coding: Autocomplete Is Free, Agents Cost You Somewhere

Every coding tool wants to hand you a “free” tier that’s actually a monthly allowance of the cheapest model they can get away with. That’s fine for autocomplete. It falls apart the moment you want an agent that actually does things: reads your repo, runs commands, iterates on failures. That work needs a capable model, and capable models aren’t what free tiers are built to hand out for free.

| Provider | Free tier (2026-08-09) | Card required | Best for |
| --- | --- | --- | --- |
| GitHub Copilot Free | 2,000 completions a month, 50 chat and agent requests a month, model auto-selected (not the frontier ones) | No | Occasional autocomplete in an editor you already have open |
| Cursor Free (Hobby) | Limited tab completions and limited agent requests. Cursor stopped publishing fixed numbers, so treat the allowance as “enough to try it, not enough to work in” | No | Same shape as Copilot, different editor around it |
| Windsurf Free | Unlimited basic tab completions, a handful of agent actions a day | No | Best free autocomplete if you code under about 10 hours a week |
| Continue.dev or Cline, pointed at a free API key | Whatever your underlying key gives you (Groq, OpenRouter, and so on) | Depends on the key | The one real path to agentic coding for $0 |

Copilot, Cursor, and Windsurf’s free tiers are all variations on the same trade: fine autocomplete, and a small allowance of agentic requests that runs out roughly when things get interesting. If autocomplete is genuinely all you want, Windsurf’s free tier is the most generous of the three because the tab completions have no monthly counter at all.

If you want an actual agent, an editor extension that reads your codebase, calls tools, and fixes its own mistakes, the free tiers baked into the coding IDEs aren’t the path. The path is Continue.dev or Cline pointed at a free API key from Groq or OpenRouter, running Qwen 3.6 or DeepSeek V4 Flash. You skip the IDE vendor entirely and bring your own model in through the back door, and the model itself costs nothing because you picked it from the API bucket above. It’s slower to set up than clicking “install extension,” and it’s exactly the kind of thing this whole article is about: nobody gives away the best agent for free, but somebody gives away the model, and you’re the one who connects the wires.

## What A $0 Stack Actually Looks Like

Put it all together and here’s a setup that costs nothing and covers real ground:

- **Daily chat driver:** DeepSeek’s web chat for anything you’d normally ask ChatGPT, no per-query counting to worry about
- **Second opinion / long context:** Gemini’s free web app when you need to dump a big document in and ask questions about it
- **Scripts and automation:** an OpenRouter key for whichever free model is current this month, with a Groq key as the fast fallback when OpenRouter’s free tier throttles you
- **Speed-sensitive stuff:** Cerebras for the two models it supports, when you want tokens streaming faster than the screen can refresh
- **Images, one-off:** Google’s Gemini app for anything you’re putting in front of readers
- **Images, batch or automated:** Pollinations.ai wired straight into a script, no key, no signup
- **Transcription:** Groq’s Whisper endpoint, no need to look anywhere else
- **Text-to-speech:** self-hosted Piper if you need it in production, ElevenLabs’ free tier if you’re just curious what a voice sounds like
- **Coding autocomplete:** Windsurf’s free tier in your editor
- **Coding agent:** Continue.dev or Cline, wired to your Groq or OpenRouter key running Qwen 3.6 or DeepSeek V4 Flash

That’s a chat assistant, a scripting backend, an image pipeline, a transcription service, and an agentic coding setup, all running on providers that never asked for a credit card. It’s not going to out-benchmark a paid Claude Opus 5 subscription on the hardest reasoning tasks, and nobody’s claiming it will. What it will do is handle the actual bulk of what most people use AI for, every day, for exactly nothing, as long as you’re willing to juggle five logins instead of pretending one vendor should carry the whole load like it’s some kind of forklift built to move a couch by itself.

## Related Reading

- [OpenRouter vs LiteLLM](https://sumguy.com/openrouter-vs-litellm/)
- [Give Your AI Agent a Cheap Intern](https://sumguy.com/cheap-workhorse-model-ai-coding/)
- [API vs Self-Hosted LLMs: The Real Cost](https://sumguy.com/api-vs-self-hosted-llm-cost/)
- [How to Stretch a Free LLM Tier](https://sumguy.com/stretch-free-llm-tier/)
- [The Free Tier Rug Pull](https://sumguy.com/free-tier-rug-pull/)