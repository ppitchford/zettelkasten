---
title: "Blog about things you don't understand yet"
source: "https://www.seangoedecke.com/blog-about-things-you-dont-understand-yet/"
author:
published:
created: 2026-08-19
---
Every post I publish represents at least two things I’ve learned: the thing that prompted me to write the post, and the thing I learned in the course of writing it. If I don’t learn anything new while I’m writing, it’s not interesting enough to publish.

Typically I learn way more than two things. For instance, in my [o3 geoguessr](https://www.seangoedecke.com/the-o3-geoguessr-prompt-did-not-work/) post, I started out with the idea that most AI prompts probably don’t work, and I ended up learning that newer OpenAI models have lost o3’s ability to geolocate. That’s interesting! In my most recent post on [C2PA](https://www.seangoedecke.com/c2pa-only-works-if-everything-is-signed/), I started out with the idea that C2PA requires near-universal adoption, but I learned a *ton* of things about PKI, managing private keys on local devices, how C2PA actually works, and so on. In my post on the [Luddites](https://www.seangoedecke.com/luddites-and-ai-datacenters/), I started out with the idea that the Luddite movement was fundamentally decentralized, but ended up fascinated by Luddite culture (which was far more elitist, misogynist, and violent than the pop-Luddism books describe). I could do this for every single post on the blog.

### Taking a position

I think the core reason this works is that **every single one of my blog posts argues a point**. I never publish a post that just gives some scattered thoughts on a topic, or a post that only says “yes, I agree with this other article”. If I write a draft that nobody sensible could disagree with, I scrap the draft. Making sure that everything I write is at least minimally controversial is a forcing function: it forces me to think about what the most interesting part of my position is, and it forces me to do enough research to defend it against the obvious criticisms.

This is contrary to a lot of advice I read about blogging, which encourages the aspiring blogger to treat their posts as a form of unstructured self-expression. If unstructured self-expression is what you want to do, that’s cool. The point of having a blog is that you get to write what *you* want. However, this advice isn’t as helpful as it sounds.

Before I was in tech, I was a philosophy grad student. But before *that*, I was a poet. One thing you learn when you try to write poetry is that it is way easier to write to a restrictive structure than it is to simply “write what you feel”. This should be obvious when you actually think about it. The task of a poet is to repeatedly choose the next word. Writing to a structure (typically rhyme or meter) narrows that choice to a small set of words, instead of the entire English language. It’s the same with blogging. Forcing yourself to write about specific, potentially-controversial points makes consistently writing easier, not harder.

### Writing, thinking, and research

**Writing is the best way to think clearly about a topic.** It’s easy to believe you understand something when you’re just turning it over in your head. When you have to condense that down into words, you find out exactly how much you do or don’t understand. I am constantly having moments where I type something, stop myself, and think “wait, that can’t actually be right”, or “is that really true?”

By the time I write my way to the end of the post, I’m usually thinking so much more clearly about the topic that my conclusion paragraph is way better than my introduction. In fact, I’ve picked up the habit of going back and immediately rewriting the first paragraph as part of my first-draft process, because I know I’m going to end up doing it anyway.

I also change my mind a lot while I write. [Here](https://www.seangoedecke.com/space-ai-datacenters-do-not-have-a-cooling-problem/) [are](https://github.com/sgoedecke/gatsby-blog/blob/2841c8504fc0b5f4dd2e8105955350bafec4d904/content/drafts/_icebox/prediction-markets-insider-trading/index.md) [a](https://www.seangoedecke.com/the-just-say-no-engineer-was-a-zirp-phenomenon/) [bunch](https://www.seangoedecke.com/giving-llms-a-personality/) [of](https://www.seangoedecke.com/ai-detection/) [examples](https://www.seangoedecke.com/tempo-faq/) [of](https://www.seangoedecke.com/impact-of-ai-study/) [posts](https://www.seangoedecke.com/ai-interpretability/) where I began writing them with the opposite opinion to the one that eventually made it into the post. I think this is a good sign, and I hope I never stop doing it. You should be researching and thinking about every post you write, and that means you should frequently learn new things that change your mind.

Because of all this, I deliberately choose to write blog posts about things I don’t yet quite understand but would like to, like [LLM](https://www.seangoedecke.com/steering-vectors/) steering, Stripe’s [Tempo](https://www.seangoedecke.com/tempo-faq/) blockchain, [C2PA](https://www.seangoedecke.com/c2pa-only-works-if-everything-is-signed/) and [watermarking](https://www.seangoedecke.com/text-ai-watermarks/), space [cooling](https://www.seangoedecke.com/space-ai-datacenters-do-not-have-a-cooling-problem/), [interaction models](https://www.seangoedecke.com/interaction-models/), LLM inference [internals](https://www.seangoedecke.com/fast-llm-inference/), and so on. This is great for me, because I learn a lot. Is it great for my readers?

### Is blogging to learn irresponsible?

I sometimes worry that I should only be writing about areas I already know very well, like [tech company dynamics](https://www.seangoedecke.com/how-to-ship/) or [working](https://www.seangoedecke.com/good-api-design/) in [large codebases](https://www.seangoedecke.com/large-established-codebases/), rather than presenting myself as an authority on fields I’m actually still learning. Should I let historians of the Luddites write about Luddism, Web3 engineers write about blockchains, and so on? I think this is acceptable for three reasons.

First, it’s sometimes easier for a beginner to write an introduction to a field than for an expert. Experts routinely [overestimate](https://xkcd.com/2501/) the knowledge of the general public, and have often internalized the reasons why their field is important so deeply that they struggle to express them. I think my [explainer posts](https://www.seangoedecke.com/tags/explainers/) are valuable because I always spend the first chunk of the post talking about *what the original problem is* before I get into the technical solution.

Second, sometimes the public consensus on a topic is just plain wrong, to the point where even a little bit of research is enough to demonstrate why. Many of my posts I’m proudest of have been along these lines: arguing that the “500ml per prompt” water usage figure for LLMs was [ludicrous](https://www.seangoedecke.com/water-impact-of-ai/), or that the popular Apple “Illusion of Thinking” paper was tracking [persistence, not reasoning](https://www.seangoedecke.com/illusion-of-thinking/), that GPUs [live longer than three years](https://www.seangoedecke.com/ai-gpus-live-longer-than-three-years/) and the AI companies have large [profit margins](https://www.seangoedecke.com/ai-inference-is-obviously-profitable/) on inference, and so on.

Third, I try to make it clear on my blog who I am and what my credentials actually are. Even if it’s not explicitly described in the post, I have my real name and resume available on my [/about](https://www.seangoedecke.com/about) page, so I don’t think a careful reader could be easily fooled into thinking I’m an expert on 19th-century England or space physics or LLM economics or anything like that.

### Feedback

Even if nobody reads what you write, writing is still a good discipline for getting your thoughts in order. But another big reason why writing is a great learning tool is that **you can get feedback**.

I think it’s obvious why this is useful, but I do want to make two points about feedback. First, if you do make your posts public, you need to have a pretty thick skin. People on the internet often fall over themselves to come up with the most cutting criticism or the harshest dunk. This goes double if you take my previous advice and try to write posts that make a clear, controversial point about a subject you’re learning. If you’re the kind of person whose whole day is ruined when a stranger is cruel to them, you might want to keep your blogging private or only share it among friends.

Second, even if your blogging is private, **you can get feedback from LLMs**. Like humans, LLMs will often give junk feedback. In my experience, OpenAI models will always tell me to moderate my claims or add caveats and hedges until I’m not saying anything at all. Sometimes their criticism will be straight-up wrong. But — particularly about technical topics — LLMs are great at pointing out areas you’ve genuinely misunderstood, and they’re far kinder than the average Lobsters or Hacker News commenter.

### Conclusion

I’m pleased and grateful that people enjoy reading my posts, but even when nobody did, I still got a lot of value out of blogging. I write as a method of thinking more clearly, as an excuse to do research on topics I want to learn about, and as a way of getting feedback.

If you’d like to try it yourself, I suggest watching for these two things. First, you should be changing your mind a lot as you write. If not, you probably aren’t doing enough research. Second, your first draft’s conclusion should be much tighter and more expressive than its introduction. If not, you probably haven’t learned anything from the writing process, which means the draft can be scrapped.

I strongly recommend this practice to anyone with an interest in writing. You will see the benefits even if you don’t publish any of your writing on the internet, particularly now that you can get good technical feedback by pasting your post into an LLM [^1].

edit: this post got some comments on [Hacker News](https://news.ycombinator.com/item?id=49293087).

---

If you liked this post, consider [subscribing](https://buttondown.com/seangoedecke) to email updates about my new posts, or [sharing it on Hacker News](https://news.ycombinator.com/submitlink?u=https%3A%2F%2Fwww.seangoedecke.com%2Fblog-about-things-you-dont-understand-yet%2F&t=Blog%20about%20things%20you%20don%27t%20understand%20yet).

Here's a preview of a related post that shares tags with this one.

> Saying the obvious thing
> 
> Stating the obvious is [surprisingly useful](https://blog.jim-nielsen.com/2026/blogging-stating-the-obvious/). Most of your knowledge lives below the threshold of conscious awareness, so it’s possible for a piece of writing to remind you of what you already know. It’s common to know you don’t like something without being quite sure why, and reading an obvious statement (such as “accuracy [matters](https://www.astralcodexten.com/p/if-its-worth-your-time-to-lie-its), even when you agree with the broad strokes”) can help clarify why you find certain things distasteful.  
> [Continue reading...](https://www.seangoedecke.com/saying-the-obvious-thing/)

---

[^1]: For what it’s worth, I’ve fiddled with careful “review prompts” and it’s basically as good to just write “review, please:” and paste your article.