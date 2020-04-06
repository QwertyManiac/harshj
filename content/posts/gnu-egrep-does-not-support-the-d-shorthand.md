---
title: "GNU egrep does not support the \\d shorthand"
date: "2015-11-01"
---

I learnt this the hard way (and said hello to a large living world of non-standard regex implementations). The [GNU grep/egrep documentation page](http://www.gnu.org/software/grep/manual/grep.html#The-Backslash-Character-and-Special-Expressions) documents all their special backslash character support but `\d` is not one of them.

<!--more-->

That is, the following will not work when trying to match a numeric digit at the beginning of file (for example):

`egrep '^\d' file.log`

You need to use either of these two alternative forms instead:

`egrep '^[:digit:]' file.log` (OR) `egrep '^[0-9]' file.log`
