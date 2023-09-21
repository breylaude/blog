---
layout: post
title: no further details
subtitle: 
gh-repo: breylaude/blog
gh-badge: [star, fork, follow]
tags: [rants]
comments: true
---

i really couldnt come up with a better title than this i just wanted something with the ancronym *“NFD”* because this is in part about normal form designators

so with that out of the way...

> i must try to keep my focus. i must try to keep my head together. i might not be there when you need me but. i must try to keep my head down.

### normal form designators

*Brian Cantwell Smith* is one of the many people who are far smarter than i am. i learnt of them through their MIT thesis Procedural Reflection in Programming Languages which is something else hoooo mama

as always, the ideas expressed through this post were taught to me first through the above paper, as such im unsure whether to attribute these ideas to Brian, but will do so as im not sure who to attribute otherwise. knowledge is a shared exercise and i welcome and thank everyone who had a part to play <3

another disclaimer but its highly likely im misusing terminology here, if thats the case please replace all occurences of phrases that appear in brians book with made up words that take up the meaning i have expressed here.

**pripl* is about the development of `LISP-3`, which is a scheme endowed with reflective capabilities. the scope of this is outside of my brain ability this blog post, but im sure i will post about that also. however, one idea that came up a lot was of normal form designators and i thought they were neat.

at its core, an NFD is the semantic grouping of the result of evaluating a statement, regardless of how far it has been evaluated.

as such, the following scheme statements all relate to the same normal form designator:

```haskell
(if true
    (* 23 (+ 4 2))
    0)

(* 23 (+ 4 2))
(* 23 6)
69
```
{: .box-note}
note that the set need not be every step of a single evaluation, the following also is part of the same NFD set:

`(- 70 1)`

### the point is

i liked this idea for a few reasons, but the reason for this post is because it lets me complain about computers and software yet again.

it feels to me like all the “““progress”““, specifically in the modern web, fails to be meaningful improvement in the sense that we arent really changing the NFD groups that the technology claims to improve.

we still use the internet to do a few basic things:

- send messages to each other
- distribute files
- view media
- 
and rather than find efficient and context focussed ways of doing so, we end up just building… larger and larger expressions that evaluate to the same thing, and claim that because its more complex for the average person to understand, its “progress”.

we still claim that “operating system improvement” is reinventing (or many times, building yet another abstraction layer on top of) the same empty signifiers of what people expect in an operating system.

we keep building up, rather than building different.

### fin

im just tired of seeing the latest new thing being some massively bloated piece of software that requires a corporation in the middle and ever more powerful computers, just to impliment the same functionality that we’ve had forever.

im tired of people being trained in the same tools industry decided is the fad this month and never trying something truly unique.

i want to find new normal form designators, and have that uniqueness being a virtue in of itself.

`C-c C-x`

