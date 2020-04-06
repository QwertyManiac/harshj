---
title: "Checking if your JRE has the Unlimited-strength Policy Files in place"
date: "2015-12-03"
---

To use the included 256-bit encryption algorithms within Java, **Oracle Java JDK or JRE** requires you to download and replace the `US_export_policy.jar` and `local_policy.jar` files under your `$JAVA_HOME/jre/lib/security/` directory with a variant of these files called `JCE Unlimited Strength Jurisdiction Policy Files` obtained [here](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html).

<!--more-->

These jars, commonly referred to as the `Unlimited JCE files`, limit which security algorithms you can or cannot use in your code.

An installation with the files **replaced with the unlimited** variants will show the below. Note specifically the permissive name of the *granted permission*:

```bash
$ unzip -c $JAVA_HOME/jre/lib/security/local_policy.jar default_local.policy
…
// Country-specific policy file for countries
// with no limits on crypto strength.
grant {
    // There is no restriction to any algorithms.
    permission javax.crypto.CryptoAllPermission;
};
```

**Note**: This only applies to Java release versions prior to *Java 8 Update 161 (`1.8u161`)*. From that version onwards, the default included policy is unlimited and no file replacement is necessary anymore on higher versions.