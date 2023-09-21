---
layout: post
title: performance test runs too short
subtitle: 
gh-repo: breylaude/blog
gh-badge: [star, fork, follow]
tags: [testing]
comments: true
---

you’ll probably notice that performance testing costs a lot of time to run one case. moreover, it carries the risk of failing the execution itself. 

furthermore, fixes to eliminate bottlenecks found in performance testing are often far more expensive than functional fixes.

i don’t know if it’s because there are few people who know this reality, or because the emphasis is on functionality, but in most cases, the performance test period is quite short.

{: .box-note}
the solution to this problem, of course, is a long performance test period.

you might hear people say, 

> “that’s difficult!” 

that is why i would like you to use the points of view that were originally not sufficient for conducting performance tests, as mentioned above, as the basis for estimates to ensure a sufficient period for performance tests.

specifically, it looks like this:

- for each case with different characteristics, one trial case is prepared, and the execution time and the time for correcting defects discovered by it are allocated. this enables subsequent performance test execution to be performed smoothly and reduces the risk of failure of the performance test execution itself.
- approximately XXX metrics and logs are required to verify the performance test results, so the environment preparation period for the performance test is required.
- this period is necessary because it is necessary not only to execute the performance test but also to carefully verify the results.
- as a result of verifying the results, problems are found, and most of the time, the correction is more complicated than the functional test, so the period for correction is *XX%* longer than the functional test.
- in particular, the fourth point should be emphasized. as i wrote in problem 1, the essence of performance testing lies in *“finding and fixing problems.”* planning for “fixing” is extremely important, but it is a step that is often neglected in any test plan.

by estimating in the performance test plan in this way, it will be easier to secure more performance test periods than before.
