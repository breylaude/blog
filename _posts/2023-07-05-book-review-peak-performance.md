---
layout: post
title: book review - peak performance v2
subtitle: 
gh-repo: breylaude/blog
gh-badge: [star, fork, follow]
tags: [book review]
comments: true
---

 a must-read for understanding system performance optimization. but it is recommended to read a book on kernel implementation first, such as [understanding linux kernel]().

the structure of this book is very clear: chapters 1 to 4 are comprehensive, summarizing the concepts of performance optimization, methodology and tool introduction; the next is basically a special explanation along the important components of the operating system: `cpu`, `memory`, `file system` , `disk`, `network`, and expanded from a single system to a cloud computing environment, and each part is refined from the four topics of concept, method, tool, and optimization direction; then it is dedicated to a few chapters to talk about more comprehensive tools : `perf`, `ftrace` and `bpf`.

it is so clear that it can be said to be a *"model book"* for performance explanations. but the tool explanations in the following are mostly listed, and it is easy to get sleepy when reading (maybe because i am not specialized in optimization), and the content related to tools is repeated. there are many narrative places, basically every chapter. if these repetitions are removed, this book should not be so thick.

so i read through the first four chapters, and roughly flipped through each of the following chapters, which is more suitable as a desk reference book.

for me, this book can also be regarded as a recap of computer composition and linux kernel. although the author talks about these things, it is very concise, but the summary is in place-these contents are quite interesting to read.

the second edition of the original book is published in 2021, and the translated version is only one year apart, and the content is still very new. according to the translator, the updated content reaches 40%+.

i haven’t read the first edition, but the translation quality of the second edition is good, but the translation of many details is very rigid, and there are many nouns with conventional translations, so I have to create one myself, and the quality of each chapter is uneven *(see errata)*, this may be a common problem of multi-person or team translation. 

another point is that i think it is better to use english directly for some nouns, but hard-translated into chinese, which sometimes makes people confused. in short, if you have a certain understanding of the kernel and if you have good english skills, it will not affect your reading, but it is best to have an english electronic version.

not surprisingly, the index was hacked.

### suggestion to publishers

- it is best to add the full name the first time you encounter an abbreviation
- the original text has the full name and abbreviation in the title, why did you omit it?
- it's better not to scan the code to get the link, it's subjective whether it's disgusting or not, but it's really inconvenient.
- do your best to add indexes, this kind of technical books involving a lot of terms, the reading experience with and without indexes is really much worse.

a book priced at 238, these requirements are not too much, right?
