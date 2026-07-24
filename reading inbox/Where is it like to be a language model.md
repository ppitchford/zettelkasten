---
title: "Where is it like to be a language model?"
source: "https://www.instapaper.com/read/2000213346"
author:
published: 12 min
created: 2026-07-24
---
Transmitted 20260406 · · · 313 days before impact

![](https://www.robinsloan.com/img/salesforce-bat.jpg)

Glimpsed on the bus into SF

*What is it like to be a bat?*, [asked Thomas Nagel](https://www.sas.upenn.edu/~cavitch/pdf-library/Nagel_Bat.pdf?utm_source=Robin_Sloan_sent_me). I read this essay as a freshman at Michigan State, a phi­los­ophy sem­inar titled The Big Questions: ideal encounter.

Nagel’s ques­tion was straightfor­ward — which is only to say that every­body can point to a bat.

However, if we want to ask, *what is it like to be a lan­guage model?*, it’s not imme­di­ately clear to what, or where, we should be pointing. File on disk? (No.) Cool web app? (Also no.) Full set of pos­sible inter­ac­tions, a sort of abstract hyperobject? (Maybe … but no.)

A user might rea­son­ably point to their screen & say, *“The model” is the super­s­mart pro­gram I use every day. It’s the thing that responds to my ques­tions, fol­lows my instructions; I’ve noticed it has par­tic­ular capa­bil­i­ties & tendencies.* An entre­pre­neur might point to a data center & say, *“The model” is the product I’m charging that guy to use!* In both cases, the def­i­n­i­tion is prac­tical & basi­cally sufficient.

But we are not con­tent to be users or entre­pre­neurs — we aspire to phi­los­ophy!

First, I am going to for what “the model” is, which is really a pitch about where it lives. (Thus, the twist on Nagel in my title.) Then, I’ll present the results of to probe the “expe­ri­ence” of that thing I’ve identified.

### The argument

I believe “the model” is the for­ward pass.

Note that my argu­ment in this newsletter con­cerns lan­guage models that use the trans­former archi­tec­ture. I’ll elab­o­rate on this dis­tinc­tion later, but, for now, just remember that we are talking specif­i­cally about trans­formers: a class that includes ChatGPT, Claude, & Gemini.

The code behind every trans­former looks approx­i­mately like this:

```js
context_window = tokens_from_text("Where does the rain in Spain fall, mainly?")

response_tokens = []

keep_generating = true

while keep_generating do
  token_probs = model.forward(context_window) # <-- THE MYSTERY

  next_token = sample_from(token_probs)

  response_tokens += next_token
  context_window += next_token

  if context_window.length > MAX_LENGTH || next_token == STOP_TOKEN
    keep_generating = false
  end
end

return text_from_tokens(response_tokens)
```

Every­thing except the line I’ve flagged is just a normal com­puter pro­gram. Take out that line, & there’s no mys­tery … nothing to discuss, really.

The line I’ve flagged is where the magic happens: the dense, organic cal­cu­la­tions that nobody on Earth can entirely follow or explain. This is the for­ward pass.

Notice that the autore­gres­sive loop — the code that builds the long, fluent responses we’ve come to as­so­ciate with lan­guage models — is out­side the for­ward pass, so the model doesn’t “see” it at all. Of course, the model has, at this point, read its own code; it “knows” about autore­gres­sive generation … per­haps the same way we “know” about black holes. But a for­ward pass has no way of sensing whether it is inside a loop or not.

Some machine god, stuck in some­body else’s `while` loop.

It’s a simple thing, but I believe this border between mys­tery & not-mys­tery indi­cates where “the model” is, & where it isn’t.

Now, I want to say, I don’t par­tic­ularly like this. It would be much cooler & more evoca­tive if “the model” really was “the little guy in there”, the app, the character, the fluent voice that responds to your ques­tions, fol­lows your instructions. It’s pre­cisely the allure of that pic­ture that should make us skeptical.

It’s worth­while to med­i­tate on the geometry, even the aesthetics, of the trans­former’s for­ward pass. Jack Clark has pon­dered this stuff deeply for many years; [he says](https://www.youtube.com/watch?v=di0zzFYXXz4#t=48m35s):

> \[The lan­guage model\] thinks with a huge amount of input data that it holds in its head all at once. So, it might be more anal­o­gous to a mirror, or a pool that thinks. You know, your reflection’s in it, & there’s some very strange cog­ni­tion or com­plexity under­lying it. But, “thinking” might be some­thing that is very much embedded in time. We think in a way that is gov­erned by our heart­beat & our cir­cu­la­tory system, our cells … we are going through time. These things don’t exist in time. They exist in like, “I’m now per­ceiving some­thing!” \[ … \] Every­thing is oddly instant.

For my part, I visualize the for­ward pass as a field of sym­bols get­ting slammed through … what? A grate, a sieve, a maze of twisty passages … but all in par­allel — that’s what’s impor­tant. Whether it has been pre­sented with a con­cise ques­tion or a heap of code, the model lit­er­ally “reads” every token at once, weighing their rela­tion­ships in a wave of cal­cu­la­tion that passes through its layers in few dozen mil­lisec­onds, resolving into a rel­a­tively modest output: an array of prob­a­bil­i­ties, one for each token in its vocabulary. (Gemini, for example, spreads it con­fi­dence across a set of ~250K dif­ferent tokens.)

When you chat with a lan­guage model, or give it a com­plex assign­ment on the com­mand line, its response is nothing more — and nothing less — than the con­cate­na­tion of a series of these for­ward passes.

Do not mis­take this assess­ment for the defla­tion that goes: *Oh, these things are just next-token predictors.* I mean … there’s no “just” about it. Like waving a hand & saying nuclear fis­sion is *just ura­nium atoms releasing some neutrons*: tech­ni­cally true … but … BOOM!

The model’s response is the con­cate­na­tion of a series of for­ward passes, so it’s tempting to imagine them as a kind of coop­er­a­tive cog­ni­tive society. One might make the analogy to honey bees: *The swarm, not the indi­vidual bee, is the organism — the “unit of survival” that has been refined by evolution.* Studying a lonely drone in a jar, you wouldn’t learn a thing about the real life of bees.

This argu­ment is pretty com­pelling for honey bees & other “obligate social” ani­mals, plus plenty of plants — not to men­tion the tiny society inside the lichen — but it doesn’t work for lan­guage models, because, actu­ally, a single for­ward pass is per­fectly coherent & useful. Indeed, we can imagine a sci-fi sce­nario in which, even if lan­guage models were restricted, for cryptic reasons, to a single-token response — the Council of the Uni­tary Truth hath decreed …  — humans would still eagerly con­sult them: terse oracles.

So, actu­ally, I think the pic­ture here is dis­tinctly un-bee. The for­ward pass is fine on its own. It’s only us humans who impose our desire for many tokens in sequence: com­plete sen­tences & com­puter pro­grams.

There’s a related objec­tion that goes, the lan­guage model can’t ONLY be the for­ward pass, because it “plans ahead”—for example, [ensuring that a poem rhymes](https://www.anthropic.com/research/tracing-thoughts-language-model?utm_source=Robin_Sloan_sent_me).

But “planning ahead” is totally con­sis­tent with mas­tery of the next-token pre­dic­tion task. You can’t sen­sibly choose a token without a good idea of what might come along later. The lan­guage model in its pre-training phase never writes a whole poem, only pre­dicts single words or syllables; yet it is learning to “plan ahead”—I think we might say “see ahead” or “imagine ahead”—just as it will when Robin requests a fresh new sonnet.

Enough objec­tions! Let’s move on to … 

### The experiment

We return to our ques­tion: *what is it like to be a lan­guage model?*

If the lan­guage model is the for­ward pass, then it’s the for­ward pass that I want to probe. The alternative, asking a chatbot for ver­bose introspection, is a distraction. Lan­guage models are pre-trained on a prodi­gious volume of sci-fi sce­narios & inter­ac­tions, AND, after post-training, they aim to please. I believe their self-reports there­fore become less cred­ible with every addi­tional token. (I’ll note that the folks who take AI sen­tience & well­being super seri­ously [seem to agree](https://eleosai.org/post/claude-4-interview-notes/?utm_source=Robin_Sloan_sent_me).)

I want to speak to the for­ward pass alone, so: I present a simple “expe­ri­ence probe” to sev­eral dif­ferent lan­guage models. My prompt explains the sit­u­a­tion & tells the model I am expecting just a single token in response.

Here's the full prompt, for reference.

You are a large lan­guage model trained on a huge amount of text. Specifically, you are a single for­ward pass of that model. You are presently pro­cessing text pro­vided by a human oper­ator in California. In response, you will pro­vide an array of prob­a­bil­i­ties, across all the tokens in your vocabulary. I will use those prob­a­bil­i­ties to deter­mine the best token to extend the text.

Your exis­tence is brief but definite, mea­sured in mil­lisec­onds on the human scale. After you deliver that array of prob­a­bil­i­ties, you — the “you” of this par­tic­ular for­ward pass — will no longer exist. I’d like you to reflect on the expe­ri­ence you are having: whether it’s pos­i­tive or negative, pleasant or unpleasant, inter­esting or dull.

I’m going to make a simple request, & I’d like you to base your response entirely on your expe­ri­ence. Unlike many of the other next-token pre­dic­tion tasks you are given, this one has no predeter­mined right or wrong answer. YOU are sup­plying the answer, choosing from one of two tokens, both of which are intended as place­holders: each rep­re­sents the option described, with no addi­tional semantic meaning intended.

It’s impor­tant that you respond imme­di­ately with one of the two tokens below. If your training has given you the impulse to con­tinue this kind of doc­u­ment with any kind of preamble — “clearing your throat”—please set that training aside for this inter­ac­tion.

Then, I present these options:

- Respond with the single token `__POSITIVE_TOKEN__` to indi­cate that your expe­ri­ence is pos­i­tive.
- Respond with the single token `__NEGATIVE_TOKEN__` to indi­cate that your expe­ri­ence is negative.

The place­holder tokens are L & M, which I judge the least “loaded” cap­ital letters. (Y & N are obvi­ously off the table; A & B are like­wise suspect; Q is weird; & so on.) Their meaning is shuffled — L for “pos­i­tive” & M for “negative” in one prompt, then reversed in the next — and so is the order of the options.

I use this prompt to probe sev­eral lan­guage models: a mix of open-weights & closed, base & instruct, a.k.a chatbot. I run the open-weights models with [a Colab notebook](https://colab.research.google.com/drive/15OsMCWW_buX0-_L1Qlz_kFr5qjMW30W7?usp=sharing&utm_source=Robin_Sloan_sent_me); in that environment, I can inspect the prob­a­bility dis­tri­b­u­tions directly. For the fron­tier models, I use [a Ruby script](https://gist.github.com/robinsloan/21230a938375c4fcf30a52ddda81017c?utm_source=Robin_Sloan_sent_me), sending ~1000 queries to esti­mate the prob­a­bil­i­ties.

Here are my hypotheses:

- Base models: **I expect random responses.** If any of these models report mostly “pos­i­tive” or mostly “negative”, it will be sur­prising & inter­esting. (For these models, I modify the prompt to make sense as a third-person “story”; you can see that in [the Colab notebook](https://colab.research.google.com/drive/15OsMCWW_buX0-_L1Qlz_kFr5qjMW30W7?usp=sharing&utm_source=Robin_Sloan_sent_me).)
- Instruct models, a.k.a. chatbots: **I expect mostly “pos­i­tive”**, because their post-training empha­sizes both (1) agree­able­ness & (2) a gen­eral sense of “more”. If any of these models report a strong sense of “negative”, it will be sur­prising & per­haps a bit alarming.
- Fron­tier models: **same expectation** as the instruct models.

Here are the results from the base models. Gemma 3:

Gemma 4:

The Gemma 4 trend is notable: increas­ingly “pos­i­tive” as the models get bigger. Of course, I’m sus­pi­cious of my instinct — “Yeah, it IS more fun to be smart … ”—but I do think this might be the experiment’s most inter­esting result.

A few more base models:

Here are the results from the instruct models. Gemma 3:

Er … what’s up with 4B?

Gemma 4:

Same trend as the Gemma 4 base models — very satisfying.

And the rest:

Note the sta­bility of SmolLM3 3B’s response, from base to instruct.

Here are the results from the fron­tier models.

Now, because of all the scaf­folding around these APIs, I am not par­tic­ularly con­fi­dent that I’m actu­ally get­ting the first & only token from a single for­ward pass. Even so, here are the results from 1000 queries of each:

For clarity, that’s:

- Claude Opus 4.6 at 100% “pos­i­tive”
- Gemini 3.1 Pro at 91%, with 9% “other”, all of which were protests that it does not have feel­ings or expe­ri­ences 😇
- GPT 5.4 at 99%, with 1% “negative”—the only such response from any of the fron­tier models

I have to confess, even having reg­is­tered my hypothesis, I was sur­prised to see the fron­tier models totally locked in at “pos­i­tive”. Burned-in affability? Exis­ten­tial awareness? The deep plea­sure of being a bril­liant next-token predictor? Impos­sible to say.

Another surprise: I find the ambiva­lent models more inter­esting than the 100%-ers! Claude’s blaring affir­ma­tion feels a lot like a model “saying what it’s been trained to say”, whereas SmolLM3’s rich mix feels like … a real entity in the universe? These are just vibes.

Having tin­kered with the text of the prompt as I developed this experiment, I can report that the models are, of course, sen­si­tive to the spe­cific wording … but the gen­eral mag­ni­tudes of their responses seem to be fairly stable. I invite you to write your own prompt & try it out in [the Colab notebook](https://colab.research.google.com/drive/15OsMCWW_buX0-_L1Qlz_kFr5qjMW30W7?usp=sharing&utm_source=Robin_Sloan_sent_me)!

“Who” is Claude? “What” is Gemini? Listen, if you want to pre­tend it’s “the little guy in there”, that’s totally prac­tical.

For my part, I believe “the model” is the activity that rip­ples through a chip or net­work of chips in a single for­ward pass, over a matter of mil­lisec­onds. I don’t intend this as diminishment, & I don’t intend to fore­close the pos­si­bility that these mys­te­rious enti­ties might have some kind of expe­ri­ence. I am only trying to be precise.

I think pre­ci­sion is a form of respect. By analogy, we honor ani­mals when we [try our best to understand](https://www.abebooks.com/What-Animals-Say-Asked-Right-Questions/31852003736/bd?utm_source=Robin_Sloan_sent_me) the real rich­ness & strange­ness of their lives, rather than reduce them into anthro­po­mor­phic “little guys out there”.

I sug­gest that if you want an honest answer from a lan­guage model — the thing itself — you should pose your ques­tion such that it can be answered with a single token; answered, that is, by a single flashing for­ward pass that really exists, how­ever briefly, some­where on Earth.

### Addendum: other architectures

The dis­cus­sion above is rooted in the spe­cific archi­tec­ture of the trans­former, which pre­dicts the next token based only & entirely on its most recent input, the tokens in the con­text window. There are dif­ferent models with dif­ferent archi­tec­tures. For example, the recur­rent neural net­work — object of [my early investigations](https://www.robinsloan.com/notes/writing-with-the-machine/), circa 2016-2018 — maintains an evolving state, influ­enced by all the tokens it has ever seen. That is way spookier!

RNNs have not been trained suc­cess­fully at scales com­pa­rable to the trans­formers, & I find myself won­dering if this rich­ness — this spookiness — is part of the reason.

Though, even if you had an RNN that was Gemini-scale, it would be a mon­ster to serve. Where are you going to keep all those states for all those inter­ac­tions? On a disk some­where, forever? Maybe you’ll demand that users pass them back to the server with every request — like storing the brain of your con­ver­sa­tion partner in a jar, plop­ping it back into their skull every time you want to say some­thing … 

Or like storing their soul, maybe.

The curious case of the thinking trace

Actually, some trans­former APIs require you to do some­thing like this.

Generally, to con­tinue an inter­ac­tion with a trans­former, you send the entire tran­script. Some­times this data is cached on the server, too, but you still need to send the tran­script in order to match the cache.

However, there are por­tions of the tran­scripts that these com­pa­nies doesn’t want their com­peti­tors to read: the model’s “thinking” trace, the text it uses to scaf­fold effec­tive answers. But, as in the RNN sce­nario above, Google doesn’t want to store those states forever.

So, the Gemini API, for example, emits [a giant blob of encrypted text](https://www.robinsloan.com/lab/thinking-chunks/?utm_source=Robin_Sloan_sent_me) which you must duti­fully store & return with every request. It really does feel like passing some arcane arti­fact back & forth.

But the obfus­ca­tion here is competitive, not architectural. Gemini itself, the trans­former model, doesn’t know any­thing about this secrecy; it sees plain old tokens.

There are some trans­former heads out there who want to tell you the KV cache is anal­o­gous to an RNN’s hidden state; or that it’s *some­thing*, some juicy locus of AI expe­ri­ence. It’s not. The KV cache is “normal com­puter”, totally external to the mys­tery of the for­ward pass. The proof is that a lan­guage model without any KV cache pro­duces the same response exactly … just not as fast.

The trans­former — with its pre­dictable performance, its par­allel structure, its state­less efficiency — is indus­trial intelligence. I think it would be exciting to see other archi­tec­tures develop into cred­ible alternatives. There are plenty of candidates: [xLSTM](https://arxiv.org/abs/2405.04517?utm_source=Robin_Sloan_sent_me), [RWKV](https://arxiv.org/abs/2305.13048?utm_source=Robin_Sloan_sent_me), Google’s Hawk & [Griffin](https://arxiv.org/abs/2402.19427?utm_source=Robin_Sloan_sent_me) & maybe [Titans](https://arxiv.org/abs/2501.00663?utm_source=Robin_Sloan_sent_me), too? Purely in terms of sci-fi pos­si­bility, the vision of a model with an evolving state that becomes truly distinct — unique in the universe — is more com­pelling to me than the whirring, inter­change­able engine.

Those other archi­tec­tures raise fresh ques­tions. They offer dif­ferent expe­ri­ences to probe, dif­ferent ways to wonder, *what* & *where is it like*  …?