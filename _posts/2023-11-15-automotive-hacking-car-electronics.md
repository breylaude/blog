---
layout: post
title: Automotive Hacking - Car Electronics
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [car hacking]
comments: true
---

Basically, they're turning into fancy computers with wheels. Soon, self-driving cars might be the new normal. Cool but the more these cars get interconnected, the more we're opening ourselves up to different digital attack surfaces. 

Whether or not you're into cars, or *into hacking cars*, or why are you here if you're not into hacking cars, I'm assuming you're into hacking cars, anyway, I'm sharing my thoughts on what the future holds for connected cars and the security challenges this represents for customers and manufacturers. I'll also share how I managed to hack my own car.

### Car electronics

Back when I was into car tech stuff (which was just last year), I remember a friend said their car computer was acting up, it meant the whole thing needed a swap. Turns out, I was way off. It's not like there's just one supercomputer ruling the show in a car; there's more to it than meets the eye.

Modern cars are like rolling computer networks, seriously. Forget about one big brain – we're talking multiple computers with specific jobs. They're all comrades, chatting away on the inside network of your car. There's a whole bunch of sensors keeping tabs on everything, from how hot it is to your tire pressure. Each of these sensors and tasks has its own little computer gang, making sure everything runs smoothly.

Check out this rundown of the **usual suspects** in car electronics. This isn't the whole shebang, and different car brands might give these systems different names:

- Engine control module (ECM)
- Transmission control unit (TCU)
- Body control module (BCM)
- Electronic brake control module (EBCM)
- Climate control unit
- Supplemental restraint system (SRS) control module

Nowadays, all sorts of vehicles use this cool thing called a **Controller Area Network (CAN bus)**. It's like the VIP club for the computer modules inside your car – they share juicy info and keep things running smoothly. 

For example, let's say your engine's having a bad day; the **Engine Control Module (ECM)** shoots a quick error code to the dashboard so you know what's up. Or, if the road's a bit too slippery, the **Electronic Brake Control Module (EBCM)** might slide a note to the **ECM** to ease up on the engine torque. It's like a digital chatroom for your car's bits, replacing the old-school hassle of wires running all over the place inside your car.

In today's swanky cars, you've got a bunch of **Electronic Control Units (ECUs)** integrating, pulling off a multitasking paragon. The car's internal network becomes a buzzing hive, with sensors and computers firing off messages left and right – we're talking hundreds every minute. Now, here's the kicker: think of this whole setup like a network, and you know what happens to networks, they can get hit. The **CAN bus**, your car's info highway, isn't off-limits to potential attacks. It's like the wild west out there in the digital frontier of your car.

Cars can be sitting ducks for trouble because their internal network is sometimes like a chic without guards. Think back to the early days of the internet, when everyone assumed all computers were on the up-and-up. Well, it's kinda like that in your car – the computers in there are ready to roll with any command they get because they assume it's coming from a friend. Guess what? that's not always the case.

Additionally, every car manufacturer uses different codes or messages in their **CAN bus** — for example, an instruction taken from the CAN bus that contains *“665 F0 16 00 00”* may instruct the car to turn on the headlights for one manufacturer, but it may crank up the radio volume for another, or simply do diddly squat. 

That's where it gets tricky for the security researchers, these instructions are often not publicly available and must be reverse engineered.

### Fin
Mixing up commands like this might add a level of difficulty for someone that may want to hack your car – you know, like slipping in a module to take control remotely using a cellular network. But let's not make fun of ourselves, it certainly doesn’t make the task impossible. Hacking into the system is still very much on the table. Additional controls must be devised and put into place to keep the hackers out.

More about hacking cars on part 2.



