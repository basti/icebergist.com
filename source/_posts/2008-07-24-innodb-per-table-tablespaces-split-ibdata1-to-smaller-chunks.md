---
redirect_to: https://www.axiomq.com/blog/innodb-per-table-tablespaces-split-ibdata1-to-smaller-chunks/
date: 2008-07-24 09:00:00 +0100
title: 'InnoDB per-table tablespaces &#8211; split ibdata1 to smaller chunks'
author: Slobodan Kovačević
layout: post
permalink: /posts/innodb-per-table-tablespaces-split-ibdata1-to-smaller-chunks
comments: true
categories:
  - Misc
---
Today I had to import 3GB of InnoDB tables in MySQL. Unfortunately, while importing the server run out of disk space &#8211; which caused whole server to grind to a halt. Naively I tried to delete imported data to free up space&#8230; I was in for an unpleasant surprise.

By default when MySQL uses InnoDB engine it stores most of the information in single file called ibdata1. One downside is that once ibdata1 file grows it cannot shrink &#8211; even if you delete all InnoDB tables. For some reason MySQL is set to use single file instead of per-table tablespaces similar to MyISAM.

Enabling per-table tablespaces is easy just add [innodb\_file\_per_table][1] to my.cnf file. Problem is that **all newly created tables**, only new tables, will be in separate files. It seems that there&#8217;s no easy way to convert old tables and reclaim the space taken by ibdata1.

There are 3 ways and two are basically export-drop-delete-import type of solutions:

1.  Convert all InnoDB tables to MyISAM
2.  Export only InnoDB tables, drop them, delete ibdata1 and import InnoDB tables.
3.  Export all databases, delete ibdata1 and import everything back.

I choose option 2 because I luckily had only 40 InnoDB tables and much more using MyISAM. For details on how to apply each solution and down/up sides of each read [MySQL: Reducing ibdata1][2].

[1]: http://dev.mysql.com/doc/refman/5.0/en/multiple-tablespaces.html "InnoDB Per-table tablespaces"
[2]: http://vdachev.net/blog/2007/02/22/mysql-reducing-ibdata1/en/ "MySQL Reducing ibdata1"
