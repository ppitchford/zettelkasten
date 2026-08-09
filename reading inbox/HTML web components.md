---
title: "HTML web components"
source: "https://adactio.com/journal/20618"
author:
  - "Jeremy Keith"
published: 2023-11-09
created: 2026-08-07
---
Web components have been around for quite a while, but it feels like they’re having a bit of a moment right now.

It turns out that the best selling point for web components was “wait and see.” For everyone who didn’t see the benefit of web components over being locked into a specific framework, time is proving to be a great teacher.

It’s not just that [web components are portable](https://www.abeautifulsite.net/posts/a-web-component-story/). They’re also web standards, which means they’ll be around as long as web browsers. No framework can make that claim. As Jake Lazaroff puts it, [web components will outlive your JavaScript framework](https://jakelazaroff.com/words/web-components-will-outlive-your-javascript-framework/).

At this point React is legacy technology, like Angular. Lots of people are still using it, but nobody can quite remember why. The decision-makers in organisations who chose to build everything with React have long since left. People starting new projects who still decide to build on React are doing it largely out of habit.

Others are making more sensible judgements and, having been bitten by lock-in in the past, are now giving web components a go.

If you’re one of those people making the move from React to web components, there’ll certainly be a bit of a learning curve, but that would be true of any technology change.

I have a suggestion for you if you find yourself in this position. Try not to bring React’s mindset with you.

I’m talking about the way React components are composed. There’s often lots of props doing heavy lifting. The actual component element itself might be empty.

If you want to apply that model to web components, you can. Lots of people do. It’s not unusual to see web components in the wild that look like this:

```
<my-component></my-component>
```

The custom element is just a shell. All the actual power is elsewhere. It’s in the JavaScript that does all kinds of clever things with the shadow DOM, templates, and slots.

There is another way. Ask, [as Robin does](https://sfba.social/@fonts/111211813228079834), “what would HTML do?”

Think about composibility with existing materials. Do you really need to invent an entirely new component from scratch? Or can you use HTML up until it reaches its limit and then *enhance* the markup?

[Robin writes](https://buttondown.email/cascade/archive/005-why-web-components/):

> I don’t think we should see web components like the ones you might find in a huge monolithic React app: your Button or Table or Input components. Instead, I’ve started to come around and see Web Components as filling in the blanks of what we can do with hypertext: they’re really just small, reusable chunks of code that extends the language of HTML.

Dave talks about how web components can be [HTML with superpowers](https://daverupert.com/2021/10/html-with-superpowers/). I think that’s a good attitude to have. Instead of all-singing, all-dancing web components, it feels a lot more elegant to use web components to augment your existing markup with just enough extra behaviour.

Where does the shadow DOM come into all of this? It doesn’t. And that’s okay. I’m not saying it should be avoided completely, but it should be a last resort. See how far you can get with the composibility of regular HTML first.

[Eric described his recent epiphany with web components](https://meyerweb.com/eric/thoughts/2023/11/01/blinded-by-the-light-dom/). He created a `super-slider` custom element that wraps around an existing `label` and `input type="range"`:

> You just take some normal HTML markup, wrap it with a custom element, and then write some JS to add capabilities which you can then style with regular CSS! Everything’s of the Light Side of the Web. No need to pierce the Vale of Shadows or whatever.

When you wrap some existing markup in a custom element and then apply some new behaviour with JavaScript, technically you’re not doing anything you couldn’t have done before with some DOM traversal and event handling. But it’s less fragile to do it with a web component. It’s portable. It obeys the single responsibility principle. It only does one thing but it does it well.

Jim created [an `icon-list` custom element](https://blog.jim-nielsen.com/2023/web-components-icon-galleries/) that wraps around a regular `ul` populated with `li` elements. But he feels almost bashful about even calling it a web component:

> Maybe I shouldn’t be using the term “web component” for what I’ve done here. I’m not using shadow DOM. I’m not using the templates or slots. I’m really only using custom elements to attach functionality to a specific kind of component.

I think what Eric and Jim are doing is exemplary. See also [Zach’s web components](https://www.zachleat.com/web/?category=web-components).

At the end of [his post](https://meyerweb.com/eric/thoughts/2023/11/01/blinded-by-the-light-dom/), Eric says he’d like a nice catchy term for these kinds of web components. In [Dave’s catalogue of web components](https://github.com/davatron5000/awesome-standalones#element-extensions), they’re called “element extensions.” I like that. It’s pretty catchy.

Or we could call them “HTML web components.” If your custom element is empty, it’s not an HTML web component. But if you’re using a custom element to extend existing markup, that’s an HTML web component.

React encouraged a mindset of replacement: “forgot what browsers can do; do everything in a React component instead, even if you’re reinventing the wheel.”

HTML web components encourage a mindset of augmentation instead.

[« Newer](https://adactio.com/journal/20636 "UX London returns in 2024") [Older »](https://adactio.com/journal/20617 "Creativity")

## Responses

### blog.jim-nielsen.com

I think the word “component” in “web components” confused a lot of people — at least it did me.

“Web components” sounded like the web platform’s equivalent to “React components”. JSX had `<MyComponent>` and now the web had `<my-component>`.

But when you try building web components the same way you build React components, it’s easy to get frustrated and give up because web components don’t work like React components — I know I gave up a few times.

[The grain](https://frankchimero.com/blog/2015/the-webs-grain/) of a React component is not the grain of a web component. Their design prioritize different functionality and forms of use. If you try to use one like the other, you’ll fight the direction of their natural grain.

Web components have their own grain and it favors enhancement over replacement. What do I mean by this?

A typical React component might look like this [\[1\]](https://blog.jim-nielsen.com/2023/html-web-components/#fn:1):

```jsx
<UserAvatar src="https://example.com/path/to/img.jpg" alt="..."
/>
```

You could write a web component this same way, e.g.

```html
<user-avatar src="https://example.com/path/to/img.jpg" alt="..."
></user-avatar>
```

But the unique power of web components (in the browser) is that they can render *before* JavaScript. React components cannot do this — full stop.

This feature of web components [encourages a design of composability](https://blog.jim-nielsen.com/2023/as-good-as-html/). Rather than an empty “shell component” that takes data and (using JavaScript exclusively) renders the entirety of its contents, web components encourage an approach of composing core content with HTML and then wrapping it in a custom element that enhances its contents with additional functionality.

```html
<user-avatar> <img src="https://example.com/path/to/img.jpg" alt="..." />
</user-avatar>
```

This specific flavor of componentization is what Jeremy calls [“HTML web components”](https://adactio.com/journal/20618):

> If your custom element is empty, it’s not an HTML web component. But if you’re using a custom element to extend existing markup, that’s an HTML web component.
> 
> React encouraged a mindset of replacement: “forgot what browsers can do; do everything in a React component instead, even if you’re reinventing the wheel.”
> 
> HTML web components encourage a mindset of augmentation instead.

I like that term “HTML web component”. It stands in contrast to a “JavaScript web components” which would be an empty element whose functionality and contents rely exclusively on JavaScript.

Per my earlier example, this would be a JavaScript web component:

```html
<user-avatar src="https://example.com/path/to/img.jpg" alt="..."
></user-avatar>
```

It relies exclusively on the presence of JavaScript and is meaningless to the end user without it.

Whereas this would be an HTML web component:

```html
<user-avatar> <img src="https://example.com/path/to/img.jpg" alt="..." />
</user-avatar>
```

It has meaning and content without JavaScript — then is enhanced by its presence.

This idea of augmentation/enhancement over replacement is intriguing.

On The Web, Augmentation Wins in the Long Run

Augmentative approaches work best on the web because 1) the web’s grain encourages enhancement to improve resilience, and 2) that’s really the best way to iteratively change something as big as the web.

Eventually all the best ideas of web-adjacent frameworks are subsumed into the platform to work in ways that augment the existing technology rather than replace it wholesale.

XHTML wanted to replace HTML4, but HTML5 wanted to augment it. HTML5 won.

Networking libraries wanted to replace `XMLHttpRequest` and their best ideas were eventually ported into the `fetch` standard — which exists in more places than just the browser these days!

The best ideas of Sass and jQuery were ported to the browser.

[Typescript’s best ideas are going to the browser](https://blog.jim-nielsen.com/2023/the-flavors-of-typescript/), but in a way that works to enhance not replace what exists.

With web components, you might even say React’s component model is being ported to the browser. But it’s being done in a way that works to enhance how the web already works, not replace it.

My takeaway is: if you’re looking for longevity, opt for a technical approach of augmentation and enhancement over replacement. The web’s grain is arranged in that direction.

## Related posts

### Installing web apps

BeforeInstallPromptEvent vs. navigator.install

### Web App install API

It’s kind of ridiculous that this functionality doesn’t exist yet.

### Applying the four principles of accessibility

Here’s how I interpret the top-level guidance in the Web Content Accessibility Guidelines.

### Hanging punctuation in CSS

A little fix for Safari.

### Of the web

Baldur Bjarnason has written my mind.

## Related links

### Web Backstories: Shadow DOM | Igalia

> Eric Meyer and Brian Kardell chat with Jay Hoffmann and Jeremy Keith about Shadow DOM’s backstory and long origins

I enjoyed this chat, and it wasn’t just about Shadow DOM; it was about the history of chasing the dream of encapsulation on the web.

### How to build a simple Camera component - Frontend News #4

A step-by-step guide to wrapping up a self-contained bit of functionality (a camera, in this case) into a web component.

Mind you, it would be nice if there were some thought given to fallbacks, like say:

```
<simple-camera>
<input type="file" accept="image/*">
</simple-camera>
```

### 005: Service workers - Web Components Club

> I strongly recommend that you read [Going Offline](https://abookapart.com/products/going-offline) by Jeremy Keith. Before his book, I found the concept of service workers quite daunting and convinced myself that it’s one of those things that I’ll have to set aside a big chunk of time to learn. I got through Jeremy’s book in a few hours and felt confident and inspired. This is because he’s very good at explaining concepts in a friendly, concise manner.

### Shadow DOM: fast and encapsulated styles – Monica Dinculescu

Monica explains how Shadow DOM could be the perfect answer for scoping CSS:

> We didn’t have style encapsulation, so we started naming things “the right way” with BEM, so that we didn’t accidentally stomp over each other’s styles. We wanted to be able to author CSS from inside a JavaScript component, so we started using CSS-in-JS. We needed all these tools, because “the platform” (read: the browsers that be) wasn’t there, and building these tools showed that there was a need to move forward. For style encapsulation, Shadow DOM *is* the platform moving forward.

Although, in a way, Shadow DOM is also another flavour of CSS-in-JS:

> Before you complain that using a Shadow DOM and Web Components means that it absolutely requires JavaScript: this is true.

### How Will Web Components Change CSS Architecture? - Snook.ca

Depending on how you’re currently structuring your CSS and class attributes, web components might not make all that much of a difference to your workflow.

## Previously on this day

### 6 years ago I wrote Bookshop

Manually machine tagging books as a kind of mindless meditation.

### 15 years ago I wrote Responsible responsive images

A future-friendly approach to mobile-first responsive design.

### 19 years ago I wrote Berlin, day 4

Less expo, more Berlin.

### 20 years ago I wrote Put your money where Joe Clark’s mouth is

Joe finds me patronising.

### 22 years ago I wrote Party on, dudes

I’m a torn geek tonight. I was tempted to stay in and watch the new show on BBC that should be called Walking With Planets. In the end, I decided to come out to a WiFi pub for a Firefox release party.

### 22 years ago I wrote Alive, alive-o!

Maybe it’s just because I was visiting the city recently, but I’m getting a real kick out of this Dublin-based blog, Twenty Major.

### 23 years ago I wrote Photoshop actions

I’ve put together a new article. It’s basically a rundown of some Photoshop actions I use to create nice photographic effects.

### 24 years ago I wrote Wow!

When I got up yesterday morning, Jessica asked me if had dreamt nice dreams.

### 25 years ago I wrote Kaese Spaetzle

Catherine, our drummer, is coming over tomorrow night. Chris and Karin are going to cook up some kaese spaetzle.

### 25 years ago I wrote Satellite on a shoestring going strong

Nasa could learn a thing or two from the U.S. Naval Academy Alumni Association. One month after the clever folks in Maryland launched a satellite built for just $50,000 (instead of the originally anticipated $1,000,000), the satellite shows no sign of deg

[^1]: I think React is trending towards becoming more like HTML over the years. Dan Abramov notes how [component composition over prop drilling](https://x.com/dan_abramov/status/1623771055943831553?s=20) is a “top react skill to learn in 2023”. Even [the react docs](https://react.dev/learn/passing-props-to-a-component#passing-jsx-as-children) specifically call out the composability of HTML and how you might want to [follow HTML’s example in your JSX](https://cdn.jim-nielsen.com/blog/2023/react-docs-composable-jsx.png)