---
layout: post
title: Counting accumulated Git changes
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [git]
comments: true
---

It's almost christmas and the team is tasked with sharing some highlights this year. We thought sharing how much code we've written is a good idea. 

Sure, it's not a great metric for measuring productivity, but it'll do for bragging rights. But.. how? `git diff --shortstat` works, however I'm not sure which commit occurred a year ago.

Not sure if this is the best method. But here's what I did.

```bash
git log --since='1 year ago' --stat | awk -F, '/files changed/{print $2; if($3 != "") print $3}' | awk -F' ' '{if($2 == "insertions(+)") added += $1; else deleted += $1} END {print "Added: " added ", deleted " deleted}'
```

### How it works
First, `git log --since='1 year ago' --stat` gives you the added and deleted lines for each commit. Then an awk script is used to extract the added and deleted chunk into separate lines. Take the output I got from my latest project as an example:

```bash
❯ git log --since='1 year ago' --stat
commit d0553a29fcc1b981ae4889d914f98b78147ab20a (HEAD -> master)
Author: laude822 <laude822@gmail.com>
Date:   Fri Dec 08 12:53:54 2023 +0800

    use smart pointer to handle connection lifetime

 examples/cadet/main.cpp     |  6 +++---
 gnunetpp/gnunetpp-cadet.cpp | 29 ++++++++++++++++++-----------
 gnunetpp/gnunetpp-cadet.hpp | 17 +++++++++--------
 3 files changed, 30 insertions(+), 22 deletions(-)

commit e6c029f58b5f7910235e4daf4fda8aded2992dc1
Author: laude822 <laude822@gmail.com>
Date:   Fri Dec 08 12:16:41 2023 +0800

    make all services non-copyable

 gnunetpp/gnunetpp-cadet.hpp | 4 +---
 gnunetpp/inner/Infra.hpp    | 3 ++-
 2 files changed, 3 insertions(+), 4 deletions(-)

commit 4275294bb3e1cbb1cbe3088a9b94f5908dc0092c
Author: laude822 <laude822@gmail.com>
Date:   Fri Dec 08 11:46:27 2023 +0800
...

❯ git log --since='1 year ago' --stat | awk -F, '/files changed/{print $2; if($3 != "") print $3}'
 30 insertions(+)
 22 deletions(-)
 3 insertions(+)
 4 deletions(-)
 23 insertions(+)
 7 deletions(-)
 ...
```

The final awk script is used to sum up the added and deleted lines.

```bash
❯ git log --since='1 year ago' --stat | awk -F, '/files changed/{print $2; if($3 != "") print $3}' | awk -F' ' '{if($2 == "insertions(+)") added += $1; else deleted += $1} END {print "Added: " added ", deleted: " deleted}'
Added: 5754, deleted: 1306
```
### Fin
I probably should have used `git log --since='1 year ago' | tail` and `git diff` on the last commit. But somehow I never go down that route.
