---
layout: post
title: Building CERN ROOT on Arch Linux ARM
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [arm, arch]
comments: true
---

{: .box-note}
Goal here is to document my process of building ROOT on **ARM64 Arch Linux**. It's quite trivial, just in case I forget how to do it in the future.

![](assets/img/bcr-1.png)

Arch Linux is running on my ARM server. which the official repository's ROOT should include. Naturally, though, it didn't. This seems that ROOT compiles properly on **x86** but not on **ARM** in some way. Unless you change the official **PKGBUILD**, anyway. Let's look into this, then.

### PKGBUILD and Ports
The **PKGBUILD** system in Arch Linux is similar to **BSD** ports. where you can obtain build scripts in bulk from a repository. If you use the right command, the programs will eventually produce an installable package for you. (Running makepkg for Arch). Consequently, the first step is to determine which repository ROOT is located in:

![](assets/img/bcr-2.png)

In the communal repository, then. Alright. Using DuckDuckGo, I discovered that the community repository is located at `svntogit/community.git`. As you clone yourself, get yourself some milk, will require some time to finish. Following that, the cloned folder should have a large number of directories.

![](assets/img/bcr-3.png)

### Building ROOT
You can now build root here by copying the root directory to a different location. But it makes sense to maintain the directories tidy. A few fixes need to be installed later in order for ROOT to build. For this reason, ROOT was never included in the official repository.

```bash
cp -r root/ /some/place/to/build/root/
cd /some/place/to/build/root/
```

Let's now alter trunk/PKGBUILD:

Change the line `arch=('x86_64')` to `arch=('x86_64' 'aarch64')` by editing it. In order to avoid the build process complaining that we are not using **x86**. Next, make all CUDA-related comments or deletions (including dependencies). Only NVIDIA's own ARM processors are compatible with CUDA. Add the `-j` flag at the end to enable parallel building.

Running `makepkg` at this point will result in errors stating that `xrootd`, `vc` and `python-pythia8` isn’t available. Copy their build scripts from the downloaded repository and build them.

```bash
cd /path/to/downloaded/repo/community
cp -r xrootd vc python-pythia8 /some/place/to/build
cd /some/place/to/build/xrootd/trunk
makepkg -siA
# And do it for all the other packages
```

{: .box-note}
Set `-DTARGET_ARCHICTURE=generic` for `vc‘s` build. Otherwise it targets **SSE** and that won’t work on a ARM.

Finally go back to ROOT’s directory. Now it’s time to patch root manually. I don’t know why. But ROOT always complains about not linking to `dl` when also linking to `sqlite3`. Run `makepkg`, wait for it to download the source tar file. `Ctrl-C` out of it. And `vim root_vx.yy.zz.tar.gz`. Vim supports changing files in tars directly. Navigate yourself to `tree/dataframe/CMakeLists.txt`. Find the line `target_link_libraries(ROOTDataFrame PRIVATE ${SQLITE_LIBRARIES})` and add `dl` to it. Then do the same to `sql/sqlite/CMakeLists.txt`

![](assets/img/bcr-4.png)

{: .box-note}
Tell ROOT to link against `dl` with SQLite

Run `makepkg --skipinteg` and wait. ROOT should built in a few hours (or a day if on a embedded board).

![](assets/img/bcr-5.png)

Finish. Use `sudo pacman -U` to install the package.

I hope this post can help someone in the future. Feel free to send me an e-mail or comment if you got any questions.
