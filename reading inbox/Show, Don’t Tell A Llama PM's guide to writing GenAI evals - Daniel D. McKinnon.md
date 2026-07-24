---
title: "Show, Don’t Tell: A Llama PM's guide to writing GenAI evals - Daniel D. McKinnon"
source: "https://www.instapaper.com/read/1997499366"
author:
  - "[[dmckinno@gmail.com]]"
published: 10 min
created: 2026-07-24
---
![](https://www.ddmckinnon.com/wp-content/uploads/2025/03/Screenshot-2025-03-30-at-6.28.13%E2%80%AFPM-1024x1017.png)

*Note: I originally wrote this post for a Meta-internal audience about a specific eval I wrote that has become canonical, but I think that even with the details scrubbed, this post still may be valuable for others.*

## Why evals?

Evals come up a lot these days for PMs. Kevin Weil and Mike Krieger, the CPOs of OpenAI and Anthropic, respectively, even said that [writing and understanding evals is the most important thing that PMs can do](https://www.ddmckinnon.com/2024/11/20/dans-weekly-ai-speech-and-language-scoop-34/). I agree.

Evals are the language of GenAI. A traditional product development cycle might look something like: PM identifies a problem → PM writes product specs → Designer creates wires → PM reviews → Engineering scopes → PM prioritizes → Engineering implements → problem solved. Different functions speak different languages and a PM translates them into the final product experience.

GenAI is different. Everyone speaks the same language: evals. This totally flattens roles and process. GenAI development looks something like: PM identifies problem → PM writes eval to capture whether model is solving problem → Engineering hillclimbs eval → problem solved.

My day job is on the Llama PM team. When a partner team wants Llama to do something, I ask them to please not send me a product spec or PRD or description of the problem. This just adds additional work for the team to parse and understand. Cut the middleman and send me an eval directly. And the magical thing is that anyone can do this.

*Note: this is written for a PM audience, but the powerful thing about GenAI is that ANYONE can contribute to the eval layer. The primary tools you need to identify problems are a deep understanding of model performance and the user problem.*

Let’s walk through how this actually works.

## Better done than perfect; You don’t need to create MMLU4YourProblem

You may have heard of famous benchmarks like [MMLU](https://arxiv.org/abs/2009.03300) or [Chatbot Arena](https://lmarena.ai/?leaderboard) that all the top labs hill climb (this is a funny GenAI word for optimizing performance on an eval) and market or even new ones like [GPQA](https://arxiv.org/abs/2311.12022), [Humanities Last Exam](https://arxiv.org/abs/2501.14249), and [ARC-AGI](https://arcprize.org/). Creating evals like these is an expensive research challenge and should be left to those who focus on GenAI evals with scientific rigor.

We are talking about something else here. We are talking about small, sometimes disposable evals that help determine if the model is solving a user problem.

Imagine you are a PM:

- on LinkedIn and users aren’t posting your suggested comments
- on Gmail and users aren’t autocompeting your suggested responses
- on X and users aren’t clicking your button to explain a post

So how do you get these problems solved? You have two options. You can either write a detailed PRD describing these problems and hand it to the modeling team and hope that they prioritize doing something about it. Or you can write your own high-quality eval and say, “please, sirs/ma’ams, make this number go up.” As someone sitting on a modeling team, I am 100% confident that the latter is both easier for you and more likely to yield results.

Imagine you listen to my advice and decide to sit down and write an eval. Read on to learn how!

## Defining the problem

The first step is to define the problem. This is the hardest part. While the academic evals mentioned earlier attempt to capture the global intelligence of the model and are very hard to design, you do not need to burden yourself with this task. Be selfish. You just think about you.

Imagine that you are interested in generating recipes from videos. You need to precisely define what makes a good recipe and how it can be measured. You might start by saying, “oh, I’ll just send the videos and AI-generated recipes to human raters and boom I’m done.” Unfortunately (actually fortunately because this is the beauty of the world we live in), humans have very diverse preferences and you are unlikely to be successful with a task like this. These human raters will have a high degree of disagreement and instead of giving the modeling team a mountain to climb, you will give them a lumpy plateau with multiple false summits.

Instead, you need to be very precise about what atomic components make up a good recipe and how you measure them. For example, a good recipe:

- is formatted nicely with ingredients up top and instructions on the bottom. You can use an AI judge to determine if this formatting is respected with high precision.
- includes all the ingredients mentioned in the video. You can get ground truth with existing speech recognition and computer vision models and use a simple string match or AI judge to confirm that all ingredients are included.
- is written in relatively short sentences, so the home chef can easily follow the instructions while food is burning and the blender is splattering tomato soup on the ceiling. This can be measured by an algorithm to count words per sentence.

I am not a chef, so these examples are fabricated, but the idea is that you break down the problem into these small, atomic components that individually can be measured with precision. Methods for measurement can be an algorithm (did the code compile? Is this math problem correct?), another AI (prompt another model, “is the answer formatted like this example?”), or even a protocol for human raters (this is very common but beware, you need to be very precise with how you run these; they are deceptively simple but practically hard).

Don’t be stingy with your evals. It is better to collect 10+ high-quality, high-confidence evals that capture various aspects of model performance important to your product than one or two that try to do too much. Your goal is a high degree of consistency. If you run the same prompt through the same model 100x with reasonable non-deterministic sampling, the eval should return a similar result each time.

## Writing the eval

Once the problem is defined, writing the eval is actually quite mechanical. The methods and labor required will vary by type. Sometimes this requires hiring raters to source and score media according to guidelines. Sometimes it involves roping a group together and recording audio. And sometimes it’s as simple as writing trivia for LLMs.

In general, as with many tasks in GenAI, try to assemble the eval with the smallest team possible. For mine, the task was straightforward. I knew exactly what we needed. I just sat down and wrote it myself. For a more complicated one assembled by a teammate, a diversity in voices was needed, so he roped in a small team. For user-scale benchmarks, I human annotators are often needed select representative examples and write golden answers because the volumes are high.

Keep in mind that the larger the circle gets, the more time you will have to spend explaining the task to others and the less likely it is that you will get what you want. I am sounding like a broken record, but many of these evals can be written entirely by a small team of FTEs and there is no need to spin up relationships with vendors, write detailed instructions, and add layers of process to QA.

*Note that GenAI evals tend to be fairly small relative to more traditional ML datasets. Evals with hundreds of questions are not uncommon, so you really can inspect each one.*

## Operationalizing the eval

Once you have the eval constructed, you need to operationalize it. There is usually a DS, DE, or engineering team that would be more than happy to implement your eval in whatever harness they use. Measuring LLM performance is very hard, so essentially every team working on these is more than happy to get a free new lens with which they can assess model performance.

After it’s in there and in front of the modeling team, the magic begins. The amazing thing about engineers and scientists in GenAI is that they love to make numbers go up, so almost inevitably by contributing an eval to the modeling loop, the model will start doing better on your use-case.

If your eval is really good, it might even be turned into a signal included in the RL phase of modeling meaning not only will the team look at it and try to make it go up but an amazing little optimizer machine will be pushing the model to perform better on your task with every round of training.

## Hillclimbing

You are the PM! Just sit back and relax while the research team toils.

Just kidding. You have to earn your keep as well. While hillclimbing, you can be most helpful analyzing patterns of wrong answers, correlating them to changes in the data mix, and sourcing new data that you think will help improve performance.

Or spend time understanding new user problems and designing new evals to measure them. Your team is probably so cracked that your old one will be saturated in no time, so be ready for what’s next.

## Appendix: The types of judges

An eval will generally be judged in one of three ways:

1. **Algorithm or tool**. This is the simplest and often the most robust. If you want your model to generate code that compiles, you run the output through a compiler and test. If you want your model to generate an answer to a math problem, you check to see if the number is correct. If you want your model to generate an answer in three sentences, you write a simple program to parse and score. This approach tends to work really well for quantitative tasks.
2. **Judge or Reward Model**. These are AIs prompted or trained to determine whether the answer is correct. A judge is very easy to create. You simply ask whatever LLM you think is appropriate to score the response if it’s good in a smarter way. A reward model is generally the same model that you might use for the judge trained to generate a score rather than response. This approach works best for fuzzy match-style tasks where the golden examples are clearly defined. It can be appealing to try this approach to substitute for generic human preference, but this often fails.
3. **Human annotators**. Humans are best for very subjective tasks like “is this response pleasant to read”, but beware, these are deceptively hard. Human evals need to be set up very carefully to achieve the level of interannotator agreement required for a good eval.

## Appendix: The unintuitive math of interannotator agreement

If you are working on a human eval, you need to very carefully scrutinize prompts, rating guidelines, tools, and process to drive a high degree of interannotator agreement. If two humans disagree on the ground truth, you can imagine that you are giving the model two objectives.

The level of interannotator agreement required is far higher than you would naively think. Imagine that you had a binary task rated in triplicate and on average two raters preferred A and one preferred B. This is good, right? 66% agreement? Wrong. This is actually only 33% agreement. While rater 1 and 2 agreed on A, 1 and 3 and 2 and 3 disagreed.

It gets even worse. Not only are you only achieving a 33% agreement rate, you need to subtract out the chances that this configuration occurred by chance. If A and B are equally likely, the agreement rate should be 50%. This means that [Fleiss’s kappa](https://en.wikipedia.org/wiki/Fleiss%27_kappa), one way to compute the quality of agreement, is actually negative, meaning this configuration where 66% of raters prefer one of two options on all tasks is lower than what would be expected by chance.

This means that there is zero value in your eval. The truth hurts. If you design a human eval, you either need far more raters or a far higher agreement rate to be able to hill climb effectively.

## Appendix: Should I trust other people’s evals (OPE)?

Never without verification. Even very popular OSS benchmarks contain errors that may or may not matter for your use-case. When someone hands you any kind of eval or judge, you must verify with your own sample prompt by hand and determine if the results make sense. I have found errors with many, many evals that my team has used and they never show up clearly in numbers. You need to go directly to the source of truth and determine if the eval makes common sense on a few examples.

## Appendix: A nice vendor hack

I mentioned earlier that you should try to use the smallest group possible to create an eval. This is one exception to this. If you have no idea where to start, there are a few white-glove LLM vendors, e.g. Scale, Surge, etc who can help. They have done this kind of work for similar companies and maybe even your exact task. If you have the budget (these vendors can be expensive), it can be helpful to ask them for a quote and a menu of options you should consider. They often can help bootstrap your work and operationalize some of the tricky bits, especially when it comes to human evals.