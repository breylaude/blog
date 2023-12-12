---
layout: post
title: FreeBSD/ARM64 in a VM on an ARM Linux Server
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [arm, freebsd, arm64, linux]
comments: true
---

[COSCUP 2020](https://coscup.org/2020/en) has ended, and I learned a lot. Among the many cool features is the ability to run **FreeBSD** on my server. Ok, inside a virtual machine.

{: .box-note}
Having *real UNIX operating on an ARM platform* is still good. 

Several helpful people from the **FreeBSD community** at **COSCUP** assisted greatly. A big thank you to everyone that helped. I'll probably never figure out how to get FreeBSD to boot in an ARM virtual machine.

Clear. I mean, I will use **KVM** to run **FreeBSD** for **Aarch64**. Using **QEMU** to run the **AMD64** version will be too simple.

Nevertheless, **FreeBSD** officially supports **QEMU** and **Aarch64** on a variety of hardware. *Because ARM is ARM*, Linux kernel configurations vary greatly amongst vendors. I still needed some time for FreeBSD to boot up properly.

Despite the fact that the document claims to have images for **Aarch64**. The official website of **FreeBSD** never contains a link to it. I had to find the file by navigating to download.freebsd.org on my own. When downloaded, decompress it.

```bash
wget wget https://download.freebsd.org/ftp/releases/VM-IMAGES/12.1-RELEASE/aarch64/Latest/FreeBSD-12.1-RELEASE-arm64-aarch64.qcow2.xz
unxz FreeBSD-12.1-RELEASE-arm64-aarch64.qcow2.xz
```

A **UEFI image** is now required. (Or U-Boot, I never bothered with it) Neither the QEMU installation nor my package manager appear to have one. One appears after some Googling.

```bash
wget https://lxr.missinglinkelectronics.com/qemu/+save\=pc-bios/edk2-aarch64-code.fd.bz2
```

Since the default size is limited to holding a minimal system, expand the `rootfs`. When the system detects an increase in disk space, it is set up to resize the root file system. I added an additional 80G.

```bash
qemu-img resize FreeBSD-12.1-RELEASE-arm64-aarch64.qcow2 +80G 
```

We can now begin booting **FreeBSD**. My parameters are essentially the same as those found in **FreeBSD**.

Booting FreeBSD. I basically follow the parameters listed in the FreeBSD documentation, and made a few small changes to get it to function on my computer (for example, virtio doesn't work for some reason).

```bash
qemu-system-aarch64 -m 4096M -cpu host,pmu=off -M virt,gic-version=3 --enable-kvm -smp 8 \
        -bios edk2-aarch64-code.fd -serial telnet::4444,server -nographic \
        -drive file=FreeBSD-12.1-RELEASE-arm64-aarch64.qcow2,id=hd0 \
        -device e1000,netdev=net0 \            
        -netdev user,id=net0
```

QEMU is listening on localhost:4444 for a telent connection. Run `telnet localhost 4444` to boot the system. And that’s it. FreeBSD is running in a VM on an ARM server!

![](/assets/img/freebsd-on-arm.png)

Remember to run `shutdown -p` to shutdown **FreeBSD**. Unlike Linux, shutdown now doesn’t perfrorm a ACPI shutdown. It just drops you into single user mode. And BSD commands tends to stop parsing arguments after the first non-argument. `rm directory` `-r` on Linux removes `directory` recursively. But on BSD it will removes `directory` and `-r` non recursively.
