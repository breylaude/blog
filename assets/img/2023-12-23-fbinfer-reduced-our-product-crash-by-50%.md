---
layout: post
title: FBInfer Reduced our Product Crash by 50%
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: []
comments: true
---

Just want to share how powerful this tool is. Infer[1], or FBInfer (since it's written by Facebook), is a static analyzer that can detect cross-function logic errors and potential data race in C, C++ and Objective-C. Which is just amazing.

[[1] Infer Static Analyzer](https://fbinfer.com/)

Update: We're currently running a C++ heavy product at Laude Technologies. Unfortunately, we run into crashes. At it's worst our analytics shows that the crash free rate is 86%. Which is unacceptable. (I know we all hate analytics, but we can't fix problems without knowing where it crashed). 

Gradually we patched more and more issues and the crash free rate raised to 91%. But then the remaining bugs makes no sense at all. Stack trace is a total wack. Nor we can reproduce them internally.

Yet, we get a lot of client complaints about crashing. At this point we tried all the more traditional options to surface problems on our environment. **AddressSanitizer**, **valgrind** and **Dr.Memory**. Non of them catched anything. 

Pesumably, we overlook some uncommon error handling or race circumstances. I also tried **CppCheck**, but once more, not much appeared.

To my dismay, I ran **Infer** over our codebase. To my surprise, this tool alerts you to a lot of potential null dereference and uninitialized values (unexpectedly, none involving races). 

We have gone over each warning individually that was patched and combined into a fresh release. We currently have a 96.1% crash-free rate! Those figures do include a few more improvements.

### How to use
Since everything is on their documents, this section should be redundant and provide more explanation. It's easy. For any CMake undertaking. By configuring the `CMAKE_EXPORT_COMPILE_COMMANDS` variable from the CLI, you can export `compile_commands.json`. Next, simply indicate it and bide your time.

```bash
$ cd build
$ cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON ..
$ cd ..
$ infer run --compilation-database build/compile_commands.json
```

That's it. A report will be generated in **infer-out/report.txt**.
