---
title: "Photon: Real-Time VLM Is Here"
source: "https://www.instapaper.com/read/2000213597"
author:
published: 3 min
created: 2026-07-24
---
## Photon: Real-Time VLM Is Here | Moondream

[moondream.ai](https://moondream.ai/blog/photon-real-time-vision-ai-is-finally-here)

Vision Language Models (VLMs) changed the game. Instead of building custom CV pipelines for every task, you can now just prompt a model about an image in plain language. That alone made vision AI easier and cheaper to adopt. But VLMs also unlocked something deeper: visual reasoning that simply wasn't possible before. Problems that were out of reach for traditional AI systems are now solvable, and almost anyone can afford to try.

The result has been an explosion of new vision AI applications. Manufacturing defect detection. Broadcast video analysis. Retail inventory and loss prevention. What used to be research-grade problems are now powering a new wave of startups, and Moondream is at the center of many of them.

But there's a gap between what VLMs can do and what they can do fast enough to matter.

Most people's experience with a VLM looks like this: you ask it a question about an image, wait a few seconds (sometimes tens of seconds), and get an answer back. The answers are often impressive. The wait is often a dealbreaker. When you're processing live video, running a manufacturing line, or making real-time decisions, a few seconds of latency kills the use case entirely.

We heard this over and over from customers. They wanted everything Moondream offers: the accuracy, the grounding, the ease of use. But they needed it faster than any VLM had delivered before.

[Photon](https://moondream.ai/p/photon) is our answer.

## Why We Could Build This

Photon is fast because it's built by the same people who designed the Moondream model. That tight coupling between model architecture, tensor shapes, and cache sizes meant we could make decisions at model design time that optimize specifically for the hardware we actually deploy on. We knew which GPU operations would matter on which chips, and shaped the model around that. These aren't optimizations you can bolt on after the fact.

On top of that, Photon runs on hand-written custom kernels tuned for each target platform. The combination of co-designed architecture and low-level kernel work is what gets us to 2x faster inference than similar-sized models on vLLM.

On an H100, Photon runs over 60 inferences per second, enough for frame-by-frame video processing. On edge devices, including older hardware constrained by supply chain realities, it still delivers usable throughput. Full performance numbers are available [here](https://github.com/m87-labs/kestrel/blob/main/PERFORMANCE.md).

Here's what matters in practice: production vision AI systems rarely run just one inference per image. You're often analyzing the same frame in multiple ways. Photon gives you the headroom to do that.

## What This Changes

Live broadcasting with real-time moderation. Manufacturing lines running at full speed with frame-by-frame defect detection. Security systems that keep pace with camera feeds. These were theoretically possible before. Now they're operationally viable.

Speed also affects cost. When you run inference faster on a GPU, each inference gets cheaper. Photon supports operation batching, which lets you trade slightly higher per-inference latency for much better total throughput. The result is that real-time image and video analysis can fit much tighter budgets than before.

## Simple, Easy Installation

Getting started takes minutes, not days:

```js
pip install moondream
```
```js
import moondream as md
from PIL import Image

model = md.vl(api_key="YOUR_API_KEY", local=True)
image = Image.open("photo.jpg")

print(model.caption(image))
# => {"caption": "A golden retriever sitting on a park bench, looking ..."}
```

Moondream is free to download and run however you want. Photon is for teams that need faster, production-ready performance. See [pricing](https://moondream.ai/pricing) for details and the [documentation](https://docs.moondream.ai/running-locally) to get started.

## What's Next

Lens, our fine-tuning product, is launching soon. More hardware support for Photon is on the way. As both products mature, they'll integrate more tightly so you can fine-tune on your data and deploy through Photon in a single step.

We're going to stay focused on making Moondream the best production-ready VLM. Faster. Less memory. Lower cost. Running everywhere.