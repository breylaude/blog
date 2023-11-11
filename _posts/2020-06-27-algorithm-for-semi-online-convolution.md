---
layout: post
title: O(nlog^2/loglogn) algorithm for semi-online convolution
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [cp]
comments: true
---

Semi-online convolution roughly says fi = ci(∑ij=1fi−jgj)fi = ci(∑j = 1ifi−jgj) recursion

Everyone uses divide and conquer fft O(nlog2n) O(nlog2⁡n) calculate right.

Probably, split it into two sections, first calculate the left side, then calculate the contribution of the left side to the right side in one fft, and then calculate the right side.

Consider splitting it into b segments each time, and then calculate the contribution between each two segments. (Here, each segment is considered to be equal in length).

The calculation of the contribution seems to be done with b^2 convolutions, but in fact, it only needs to be done to record the point value of each segment and the point value of the transfer sequence.O(b)O(b)Second-rate DFTDFT.

The complexity is probablyT(n) = bT(nb) + O(nb)+ O(nlognb)T(n) = bT(nb) + O(nb) + O(nlog⁡nb).

When b = logn b = log⁡n, the complexity is O(nlog2nloglogn) O(nlog2⁡nlog⁡log⁡n)

### Implementation details

Generally, it is faster to take 4, 8 or 16 for b.

When partitioning, you can take 2^x in the first few paragraphs first, so that it is convenient to directly fft without wasting the length and it is convenient to memorize the point value sequence of the transfer array.

The actual operating efficiency is much faster than the traditional divide and conquer fft writing method

I posted my code on Github because it's a bit long

The actual efficiency does indeed beat the traditional divide and conquer fft.

