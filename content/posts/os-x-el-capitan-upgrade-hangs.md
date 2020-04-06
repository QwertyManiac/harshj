---
title: "OS X El Capitan Upgrade Hangs"
date: "2015-10-02"
---

Just like it did when I upgraded from **OS X Mavericks** to **OS X Yosemite** a year ago, attempting an upgrade from **OSX Yosemite** to **OSX El Capitan** hung again in the same nondescript manner - a blank/empty screen with the pointer, for seemingly infinite period.

<!--more-->

I tried to wait it out for a few hours (surely a 30% filled SSD shouldn't require hours?), but that made no difference. The good bit is that this state is easy to recover from - just hold your power button down until the machine powers off, and then restart it (it will go back into **OSX Yosemite** or whatever old OS state you had running, without issues)

I ended up recalling the fix I did the last time, i.e. moving `/usr/local` out of the way, given I'm a heavy [Homebrew](http://brew.sh) user, and gave it a try again and the upgrade succeeded in under 20 minutes in the subsequent attempt.

Before the upgrade, I ran in the `Terminal.app`:

```bash
sudo mv /usr/local ~/local
```

And after the upgrade (with a `brew update` command just to be sure it is not broken after the move-back):

```bash
sudo rmdir /usr/local && sudo mv ~/local /usr/
brew update
```
