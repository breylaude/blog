---
layout: post
title: Clean Code is Trash
subtitle: 
gh-repo: breylaude/blog
gh-badge: [star, fork, follow]
tags: [code, maintainability]
comments: true
---

I hate this book.
I hate this book long before it was trendy to hate this book.
I gave it a lot of thought for a very brief moment of time, and then it became clear to me that this advise is bad. 
I mean it's just harmful. 

Ok, with regards to the majority of the advices, this pertains to "Clean Code" in particular. 

A bunch of other things, lots of design patterns, they're highly specific to a certain language, and a lot of principles that they talk about that are important are things that are not important in other languages and are not relevant in other languages. 

I'm not saying I'm the best in the field but I have certain thoughts that I believe are far more relevant because I've worked in a bunch of different languages and I've worked in a bunch of different frameworks and I have found the same techniques to be useful over and over. 

Things like "Clean Code" do nothing but stir up arguments, opportunity for teams to argue with each other and for people to point to things and to tell each other that they're wrong and it just creates contention in a group and that's pointless especially when a lot of the advice actually leads you the wrong direction. 

### "Maintainable" is judged by people other than the programmer writing it. 

First of all, the point of code maintainability is to make the code easier to work on for someone who's unfamiliar with it, not to make it easier for you to write in the first place. 

There's a lot of speculative garbage in the advice on how you write code. It's not about some hypothetical "how changes might get made in the future." It's about "how do you find the places that changes need to be made now" and "how do you assure that you're correct in that" and that those changes are isolated. 

It's not only how simple it is for someone else to continue working on your project; if you've been doing 
this for some time, that you're almost always "Maintainable" code comes in handy when you take a break from a project and do something else for a while and then you come back to it and have to solve the problem from scratch. 

It's not uncommon for me to say, "Okay, who idiot wrote this?" "Oh look, I did six months ago." Code isn't read top to bottom like a book. Code is generally not read. A file is not something you open at the top, read down, and then go to 
scroll down to the next file. Not if you have a choice at least. If reading it like that is your only means of comprehending a code base, if it is, then the original programmer failed. Apart from that, if given the choice at all, I never read a piece of code from top to bottom. Here's where the majority of literature completely falls short.

I am aware of how difficult it is to write code listings that are legible enough to be printed on dead trees and bind into books so that they can be understood. Getting it to work that way is challenging and requires talent, but if you 
consider that it makes sense to optimize your code such that everything is contained in two or three lines, and that the typesetter hasn't made it easier for you to decide where to place page breaks if something that should take up half a page instead ends up taking up three pages. I think you are valuing the wrong thing. So here's a listing from "Clean Code". 
Pages 30 and 31. 
