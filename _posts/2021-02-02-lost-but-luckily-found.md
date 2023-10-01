---
layout: post
title: lost and found
subtitle: 
gh-repo: breylaude/blog
gh-badge: [star, fork, follow]
tags: [backup, scripts]
comments: true
---

Hey, so the other night, I was knee-deep in cleaning up my disk partition, trying to create some breathing space amidst the chaos of auto-generated files, the secret world of makefiles, and other classified stuff.

{: .box-note}
Picture this: I'm cleaning, and suddenly, in a new terminal tab, I get hit with this bombshell:

```bash
-bash: home/laude/src/utils/oracle.utils.sh: No such file or directory.
```
damn, things just got real.

At that point, shit kicks in. I mean, seriously, now I have to rewrite all those commands I had carefully crafted? 

>No way! Respawn respawn

I contemplate giving up and heading downstairs for a comforting cup of milk.

As I sip my milk, trying to console my tech-weary soul, it hits me like a ton of bricks – my terminal from the cleaning session is still open! Hope flickers back to life.

{: .box-error}
So, what's the magic solution? 

I stumble upon the `declare` method. Turns out, if you want your precious functions back, you just need to type `declare -f` And those vanished variables? Easy peasy lemon squeezy, use `declare -x` 

Oh, and don't forget your aliases, you can reclaim them with a simple `alias` command.

### And the grand lesson from this?

Always, and I mean always, back up your scripts.
