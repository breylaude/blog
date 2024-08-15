---
layout: post
title: PLC's operating systems
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [operational techology]
comments: true
---

Recently I was asked *“How could a hacker possibly attack an industrial controller like a PLC or SIS, since there is no operating system in these devices?”*

Now some manufacturers would like people to believe there is no operating system in a controller, but unfortunately this is not true. Every **RTU**, **PLC**, **SIS** or **DCS** controller on the market today has a commercial operating system in it. 

For example, here are just a few I have worked directly with lately:

|PLC |Operating System|
|-----|--------|
|Allen-Bradley PLC5|Microware OS-9       |
|Allen-Bradley Controllogix  |VxWorks      |
|Schneider Quantum  |VxWorks      |
|Emerson DeltaV     |VxWorks      |

Now while these operating systems are not famous like Windows or Linux, they are definitely commercially available operating systems. And like the desktop operating systems, they have bugs and vulnerabilities. For example, this **VxWorks Vulnerability Notice** from the **US-CERT** specifically lists Rockwell and Siemens products that are affected. Obviously, it should have listed Schneider and Emerson too.

### PLC Operating System Exploits Abound

There are many vulnerabilities like this – go to [http://www.kb.cert.org/vuls/html/search](http://www.kb.cert.org/vuls/html/search) and enter your favorite vendor in the search box and you will find many. And this is only a small part of the list, that is, the vulnerabilities that have been publically disclosed. I know many more that are not listed, such as the vulnerability used in this **MTL Instruments** security video.  In this video, a worm on a USB key attacks a PLC over Modbus TCP using a zero-day PLC vulnerability.

With these commercial operating systems and public vulnerabilities also comes free hacker tools. For example, if you want to exploit the VxWorks vulnerability I listed above, check out this [dark READING blog article](http://www.darkreading.com/blog/227700848/vxworks-vulnerability-tools-released.html).

And if you can’t find the free tool for the control system you want to attack, you can always purchase one. In a March blog article I discussed how a Russian company, GLEG Ltd, is selling the *“Agora+SCADA”* exploit pack. The pack contains 23 modules for attacking systems by various manufacturers – including eleven zero-day exploits. It costs $4500 – perhaps a lot for a teenager, but peanuts for a criminal gang wanting to extort money from a power company.

### Protect them with Firewalls

Until recently, few people knew about these PLC vulnerabilities and attack tools. Like the operating systems they exploit, they were not famous like Windows vulnerabilities are. This all changed when Stuxnet came out – now every hacker in the world knows about **PLCs**, **HMIs** and the opportunities to attack them.

Worst still, Stuxnet showed the world that finding a zero-day vulnerability isn’t even required to attack most **PLCs** – you just need to get a foothold in a computer that communicates to a PLC. Furthermore, the internals of Stuxnet and how it attacks a **PLC** is well explained on the Internet and the concepts can be modified for reuse on other systems. Joel Langill, Andrew Ginter and I detail why we believe this will happen in the Looking Forward section of our White Paper “How Stuxnet Spreads”.

Of course the next attack will not be exactly like Stuxnet. Stuxnet is only a beginning to what we will see in the next few years. It is time to start protecting our industrial controllers. There is no silver bullet, but I believe that shielding them with firewalls from all other equipment on the network, including the **HMIs**, is an important start.
