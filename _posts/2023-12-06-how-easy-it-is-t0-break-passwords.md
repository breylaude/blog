---
layout: post
title: How easy it is to break (weak) passwords
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [cryptography]
comments: true
---

A friend sent me an encrypted ZIP file and says 

*This ZIP file contains some artwork, but the password is distributed in a past, live event. And it's the birthday of the artist. 8 digits. Can you crack it?* 

Aparrently, he can't find the password. *Sure* I said. I've learned in my cryptography and security class that 8 digit passwords are really easy to crack. Worst case, I can iterate through all the possible combinations with **BASH** within a day. But I've heard this **John the Ripper** thing is really good at cracking passwords. So I decided to give it a try.

I dumped the hash with `zip2john file.zip > password.hash` and started cracking it with `john password.hash`. First I saw john decided to use the CPU only and, well I'll have to go to bed and see what happens tomorrow.

```bash
> ~/Documents/not-my-projects/john/run/john password.hash
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 16 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
```

But just a few seconds, just before I lunch `htop` to see what's going on, I see this:

```bash
Almost done: Processing the remaining buffered candidate passwords, if any.
```

Then immediately, (I removed the password and the filename):

```bash
0g 0:00:00:03 DONE 1/3 (2023-08-03 01:24) 0g/s 16334p/s 16334c/s 16334C/s Ztail1900..Uzip1900
Proceeding with wordlist:/home/marty/Documents/not-my-projects/john/run/password.lst
Enabling duplicate candidate password suppressor
xxxxxxxx         (somefile.zip/xxxxxxx.png)
1g 0:00:00:20 DONE 2/3 (2023-08-03 01:24) 0g/s 2559Kp/s 2559Kc/s 2559KC/s 16212908..Nicole8208
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

I got the password! It took just 20 seconds to crack an 8 digit password. On an old workstation CPU. I'm not even using a GPU!. And I'm not even using a personal wordlist. I'm brute forcing.

Just imagine that password is the only thing holding attackers back. Even though you use strong password and have a password manager. I'm legit scared by how fast I, *a cracking amateur*, can break it. I certainly hope that every password in existance are much stronger than this.

That is only until I realized, Credit/Debit card PINs are 4 to 8 digits long. I know, it's different as the secure element prevents you from trying more than 3 times or dumping the hash. But still, can we have actual, good authentication? Like TOTP or U2F?



