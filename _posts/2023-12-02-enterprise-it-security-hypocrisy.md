---
layout: post
title: Enterprise IT Security hypocrisy
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [security]
comments: true
---

I've been debuging issue with some clients. We recently updated how our virtual camer works on MacOS. Before we use the **DAL interface** and now switched to the more secure system extension. **DAL** works more like how Windows implements virtual camera using **Direct Show**. 

The system loads a dynamic library into apps that wants to use it. There are obvious problems, any **DAL plugin** can execute arbitrary code at that point. Including reading the process's memory and steal confidential information. Also, since **DAL** is executing as another app, you need a `daemon` to transport video frames from source to destination.

So Apple introduced the new *API, System extensions*. It runs the extension in it's own *sandbox* (ignoring technical details here) and the system does the **IPC**. Basically a micro-kernel design so app developers can have low level access to the system while keeping system integrity. Extension can't read anything it shouldn't. After we released this upgrade, some of our enterprice customers started to complain that they can't install the system extension. We did some debugging with them. Turns out **MDM** software can and commonly will block 3rd party system extensions from loading.

==> [Apple, System Extensions](https://developer.apple.com/documentation/systemextensions)

{: .box-note}
This is already stupid. They are actively saying *you should totally use the old API that let's 3rd party load arbitrary code into your browser*.

Now for the more stupid part. In the **DAL** approach, due to the need to support multiple users and deal with app sandbox, we are forced to run our transport daemon **AS F**KING ROOT**. And **MDM** vendors allowed it. Yet they complain system extension is too dangerous to allow because it directly interacts with the kernel? We can already wreak havoc many, many times and even kill your **MDM** process, heck brick the entire OS. Sure you have kernel level access, but that doesn't stop anyone from doing stupid stuff with root power.

{: .box-warning}
This is why I will never work for some shit enterprise. Not until management have received proper information security training.

As a side note. Stealing documents from a **MDM-ed** machine is easy. Ever tried bitbanging through the audio jack? It's slow, easy, fun, doable with tools preinstalled on the two relevant enterprise desktop OS and makes manager's jaw drop when I returned 30 minutes later with their top secret. No, blocking USB, forcing VPN and demanding strong passwords does not stop a sophisticated attack with physical access.
