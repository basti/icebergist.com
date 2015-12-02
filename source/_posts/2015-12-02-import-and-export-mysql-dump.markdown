---
layout: post
title: "Import and export MySQL dump"
date: 2015-12-02 10:44:23 +0100
comments: true
author: Slobodan Kovačević
categories: 
  - Misc
tags:
  - mysql
  - howto
  - linux
  - tips
published: true
---

Another simple task that's often hard for beginners is importing and exporting MySQL dumps. Here is quick rundown on how to do it.

To export data you need to use `mysqldump`:


```sh
mysqldump -u db_user -p db_name > dump_name.sql
```

Options given to `mysqldump` are:

* `-u db_user` - connect as user `db_user` to database
* `-p` - use password, it will ask you to enter your password
* `db_name` is the name of MySQL database you want to dump
* `> dump_name.sql` - by default `mysqldump` will print out the dump to terminal, but simple output redirect with `>` will instead write it to given filename, in this case `dump_name.sql`

Now that you have `dump_name.sql` file with all SQL queries needed to replicate your database you can import it using general-purpose `mysql` client:

```sh
mysql -u db_user -p db_name < dump_name.sql
```

User, password, and database name options are the same as for `mysqldump`. Since `mysql` reads input from terminal this time we can use `<` to read input from given file instead.

As always for more information you can consult manual using `man mysqldump` and `man mysql`.