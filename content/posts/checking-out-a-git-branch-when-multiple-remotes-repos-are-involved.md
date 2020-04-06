---
title: "Checking out a git branch when multiple remotes (repos) are involved"
date: "2015-09-30"
---

When you are dealing with a local git repository which has multiple remotes defined (i.e. has multiple remote repositories possibly carrying the *same branches*, such as *multiple forks*), then the right way to checkout a working branch from one of them is to use the below syntax:

```bash
git checkout -b master remote-name/master
```

<!--more-->

Where *remote-name* is the name of the remote added via a `git remote add` command earlier.

You can also delete a local branch before switching to another repository's copy of it:

```bash
git checkout origin1/master
git branch -D master
git checkout -b master origin2/master
```
