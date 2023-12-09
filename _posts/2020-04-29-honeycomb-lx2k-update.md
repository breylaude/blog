---
layout: post
title: HoneyComb LX2K Update
subtitle: 
cover-img: /assets/img/honeycomb-lx2k.png
thumbnail-img: /assets/img/honeycomb-lx2k.png
share-img: /assets/img/honeycomb-lx2k.png
tags: [arm, honeycomb lx2k]
---

![](/assets/img/honeycomb-lx2k.png)

My previous post on my **ARM workstation/server** was two months ago. I think it's time for an update on how things are progressing and if there are any issues. 

No, not at all. There are no severe problems with the board. **NXP** and **SolidRun** did an excellent job in developing it.

Without a doubt, the board is extremely efficient. My board serves as a *video streaming server*, a *data analysis server*, and an *ARM build server* for my personal lab. 

Nonetheless, the performance characteristics are markedly different when compared to a similarly configured **spec-ed x86**.

Openbenchmarking shows the **LX2160A** is at least comparable with **Ryzen 2400G**. But in my experience it really depends on what work load it is doing. 

I figured **LX2160A** struggles to transcode **4K 60FS H256** into **1080p** in realtime and run **scikit-learn** on a production dataset and **HTM** calculations. But it is totally a beast when doing **FFT** and raytracing. 

I can only guess this have something to do with the cache and branch prediction. It seems to struggle when decision making and memory access is the bottlenek while very good at pure number crunching.

### Building missing Pkgs

{: .box-note}
**Arch Linux ARM** has done an outstanding job of maintaining and developing **Arch Linux** on **ARM processors**. 

They immediately grab the buildscripts from **Arch Linux** and build them for **ARM**. However, this process is not automatic for all programs. Some packages are setup by default to build only on **x86**, and flags must be adjusted to build on **ARM**. 

As a result, the package is not in the **ARM** repo. One of these is `cern-vdt`, which is set up to vectorize using **SSE**. 

Obviously, that will not work on **ARM**. *The solution is straightforward*. Arch Linux's repository is actually just a collection of **PKGBUILD**s that you utilize in the same way that **AUR** is used. Grab the **PKGBUILD** and patches, make a few changes, then run `makepkg -si`. Done.

Fortunately, not many of the packages I want must be built in this manner. But I ran into a nasty linking problem while attempting to get **ROOT** to compile as a package (so I can finally get rid of my "launch a shell in **systemd** to load config else **ROOT** won't work" hack). 

The **PKGBUILD** for **ROOT** always creates a new directory in which to build. Making the problem more difficult to solve because it takes more than 30 minutes to get to the error and I have to clean build every time. So I devised a workaround: I removed the line in url and `xxxsums` that refers to the source code.

![](/assets/img/root-pkg-on-arm.png)

**ROOT** package on ARM!

### Security
After a few days, I began to realize that **fail2ban** was not performing as intended. `REJECT` was not supported by `iptables` or the kernel. Setting **fail2ban** to utilize `DROP` resolves the problem. Though I've never determined what causes this, I'm confident that every command is enabled in the kernel.

Also. As much as I'd like to have a **MAC** like **AppArmor** to protect the system from **ACE** in the event that one of the services fails. Mainline **Linux 5.6** still won't boot on the board - I'm stuck at **4.19**. And I'm not interested in starting from scratch with **AppArmor**. 

For the time being, I'll have to rely on standard UNIX user and group permissions.

