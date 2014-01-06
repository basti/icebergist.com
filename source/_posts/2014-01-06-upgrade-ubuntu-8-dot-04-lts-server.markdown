---
layout: post
title: "Upgrade Ubuntu 8.04 LTS server"
date: 2014-01-06 11:36:43 +0100
comments: true
author: Slobodan Kovačević
categories:
  - Misc
tags:
  - ubuntu
  - howto
  - tips
  - hosting
---

Note to self: here is how to upgrade Ubuntu 8.04 LTS (or any other release that is no longer supported) to newer Ubuntu release.

When you are upgrading unsupported release of Ubuntu if you try to do the usual `sudo apt-get update` it will most likely fail because... well, it's unsupported. The simple fix for this is to change your `/etc/apt/sources.list` and replace repository URLs from something like `us.archive.ubuntu.com` to `old-releases.ubuntu.com`.

After that you should be able follow normal upgrade procedure (use sudo if you are not root):

``` sh
apt-get update
apt-get install update-manager-core
do-release-upgrade
```

References:

* [Rimuhosting's page on upgrading Ubuntu][1]
* [Discussion on upgrading unsupported Ubuntu release][2]

[1]: http://rimuhosting.com/knowledgebase/linux/distros/ubuntu "Rimuhosting's page on upgrading Ubuntu"
[2]: http://askubuntu.com/questions/91815/how-to-install-software-or-upgrade-from-old-unsupported-release  "Discussion on upgrading unsupported Ubuntu release"