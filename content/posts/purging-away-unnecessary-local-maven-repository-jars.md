---
title: "Purging away unnecessary local maven repository jars"
date: "2015-09-22"
---

As someone who works with a lot of Java (or other JVM related) projects at work, I often notice my **$HOME/.m2 local maven cache** grows quite big over time. This is due to a natural result of a lot of `mvn install` and other equivalent commands, that build and install the project and recursive dependency jars locally.

<!--more-->

More often than not, when working on such projects, the version is usually set to a **SNAPSHOT** styled one, for example `spark-streaming-flume_2.10/1.5.0-SNAPSHOT`, and these can be deleted away when you are no longer working on the project. Here's the simple command I use to erase them away:

```bash
find $HOME/.m2 -type d | grep 'SNAPSHOT$' | xargs rm -rf
```
