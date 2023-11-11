---
layout: post
title: catch game
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [cp]
comments: true
---

### Day 4 individual contest problem b

Description: In an N*M matrix, there are two people AB in two different positions. AB walks in turns and can take any number of steps up, down, left, and right within the scope of the board. If a person stops or walks through the row or column where the opponent is currently located, the opponent wins. Given the size of the matrix and the initial position of AB, A goes first. If AB goes as good as possible, ask who will win.

### Analysis

Without loss of generality, the positions of AB are respectively (Ax, Ay) (Bx, By), Ax<Bx, Ay<By (because if the less than sign does not hold, just flip the matrix accordingly) Set x=bx- ax y=by-ay, it can be proved that the translation of AB to (0, 0) (x, y) is equivalent to the original position: if A moves up x1 step, B moves up x1 step; A moves to the left Move y1, B also moves y1 steps to the left, so that B can force A to (0,0) in the upper left corner and at the same time fall on (x,y); if A does not go up/left, then B You can control the distance between A and yourself in a smaller range. If A cannot guarantee to win, this will only make A lose faster, which can be seen below. Next, we will discuss when A is at (0,0) and B is at (x,y), A will go first, who will be the winner. First of all, it can be deduced from the simplest.

If x=y=1, then obviously B will win.

If x>1 y=1, then A obviously has to win: A walks to (x-1,0). According to the above, A can push B to the lower right corner, so A wins. Change to x=1 y>1 and A wins.

If x=y=2, then if A goes one step down, B goes one step to the left; or A goes one step to the right and B goes one step up. So A can only go back, B keeps up, and returns to situation 1. ……If you derive a few more, you will find that, in fact, all cases can be classified as a. x=y and b. x!=y. Two cases a. x=y, if A goes down n steps, B goes left n steps ; Or A moves n steps to the right, and B moves n steps up. So B can force A to (0,0), and he is in (xn, yn), and it will go back to case 1, and B wins. b. x!=y, assuming x>y, then A can go to (xy, 0) and B is in (x, y), then B must go first, which is equivalent to the case of AB swapping names a, A win.

### Solution

```c
if(abs(Ax-Bx)==abs(Ay-By){
  printf("A will win.\n");
}else{
  printf("B will win.\n");
}
```
