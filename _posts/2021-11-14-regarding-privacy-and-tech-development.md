---
layout: post
title: Regarding privacy and tech development
subtitle: 
gh-repo: breylaude/blog
gh-badge: [star, fork, follow]
tags: [privacy]
comments: true
---

{: .box-note}
Modern software (especially from corporations) are a privacy concern...

Why in the world would a video conferencing program wants to know the spec of my PC and when I'm using it? And it's never opt-out. I think this in an unfortunate fact of competition between tech companies for their market dominance. And users collectively decide and pay for UX.

Let's say *Vidz* and *Boom* are two video conference program competing in the market. Both wish to squash their competitor. So both have to push as much feature as possible and provide a better experience then the other option. 

Think what you'll be doing when you are forced to cut corners. You'll likely not do all the research needed before implementing said feature. Ignored a few checks, didn't read some important notes on the SDKs documents, copy code off StackOverflow without actually reading it... And that leads to bugs. 

Worse, these bugs only show up on the user's computer since you'll already fix them if the pasted code failed on your box. How are you, as a developer will go about diagnosing bugs that the users encounter? Collect information of course. Maybe it's caused by a specific GPU model? A certain Windows version? No one knows becauses no one is careful when coding. 

Then finally through the telemetry the missed detail is rediscovered. It's the same story for UX. The easiest way to know what users wants is to look at their use pattern.

{:.box-warning}
Unfortunately, the market prefers these bad engineering efforts. 

The first to push features and build a *better* product wins. Not the more ethical one. The game isn't about winning in the long run either. Even if good engineering practice will win over the long run. Investors are not patient; they'll force features into the project or pull their investment out. 

{: .box-error}
Either sabotage good engineering or killing the entire company.

This is why I think most open source projects are actually better software compared to the close sourced counterparts. Being open source means that no one, not event investors can force crappy, not well though-out features into the codebase. 

The potential backlash is not worth the benefit. Besides in case of investors/managers screwing up the codebase. People can fork and put a big middle finger to them.

**From my experience working at a startup.**
