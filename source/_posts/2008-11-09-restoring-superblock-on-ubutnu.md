---
date: 2008-11-09 09:00:00 +0100
title: Restoring superblock on Ubutnu
author: Slobodan Kovačević
layout: post
permalink: /posts/restoring-superblock-on-ubutnu
categories:
  - Misc
tags:
  - linux
  - ubuntu
---
Recently I had a problem on my Torrent box (an old PC that I use as dedicated torrent client) that runs Ubuntu. For some reason my root partition was being mounted as read-only. Everything else seemed to work (all other partitions were mounted properly), but I couldn&#8217;t change any of my config or do anything on root partition.

I did the usual stuff:

*   Run fsck checks and it said that everything is fine
*   Used Ubuntu&#8217;s live CD to boot, which got me read-write access to root partition. I changed some things in fstab, tried to get it to be rw permanently. No matter what I did as soon as I rebooted the root partition was once again read-only.
*   I tried booting from some repair disks I have, but all checks passed and no problem was detected. <img src='http://icebergist.com/wp-includes/images/smilies/icon_sad.gif' alt=':(' class='wp-smiley' /> 

Finally, I read somewhere that a similar problem was caused by faulty superblock on hard drive. Fortunately Ubuntu stores superblock backups in different places around disk, so I decided to try to restore it from one of those backups.

It turned out that all I needed was a single command (this [Ubuntu forum post helped][1]) to restore superblock:

`e2fsck -b 32768 /dev/hdc1`

After that my root partition was back to read-write mode. <img src='http://icebergist.com/wp-includes/images/smilies/icon_smile.gif' alt=':)' class='wp-smiley' /> 

Before you do stuff like that to your computer I suggest that you read man pages for [mke2fs][2] and [e2fsck][3]. It will prevent you from doing something foolish like deleting your whole hard drive. <img src='http://icebergist.com/wp-includes/images/smilies/icon_smile.gif' alt=':)' class='wp-smiley' /> 

[1]: http://ubuntuforums.org/showpost.php?s=72da065bbe1506b27f41a8cfc252c732&p=1424786&postcount=5 "Bad superblock"
[2]: http://linux.die.net/man/8/mke2fs "mke2fs"
[3]: http://linux.die.net/man/8/e2fsck "e2fsck"
