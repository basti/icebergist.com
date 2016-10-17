---
layout: post
title: "Backing Up and Restoring a PostgreSQL Database"
date: 2016-10-14 14:05:38 +0200
comments: true
author: Jovana Dačić
categories: 
  - Ruby and Rails
tags: 
  - ruby
  - rails
  - PostgreSQL
published: false
---

#Backing Up and Restoring a PostgreSQL Database

While working on different projects and in different environments, we often need to export a dump from one database and then import it into another. A while ago [Slobodan](http://http://orangeiceberg.com/about/ "About Slobodan") wrote how to [export and import a mySQL dump](http://icebergist.com/posts/import-and-export-mysql-dump/ "Import and Export mySQL dump"), and here is a guide how do it for PostgreSQL.

##Exporting a PostgreSQL database dump

To export PostgreSQL database we will need to use the [pg_dump](https://www.postgresql.org/docs/current/static/backup-dump.html "PostgreSQL" ) tool, which will dump all the contents of a selected database into a single file.
We need to run `pg_dump` in the command line on the computer where the database is stored. So, if the database is stored on a remote server, you will need to SSH to that server in order to run the following command:

```
pg_dump -U db_user -W -F t db_name > /path/to/your/file/dump_name.tar
```
Here we used the following options:

* `-U` to specify which user will connect to the PostgreSQL database server.
*  `-W` or `--password` will force pg_dump to prompt for a password before connecting to the server.
*  `-F` is used to specify the format of the output file, which can be one of the following:
	* `p` - plain-text SQL script
	* `c` - custom-format archive
	* `d` - directory-format archive
	* `t` - tar-format archive

<sup>*custom*, *directory* and *tar* formats are suitable for input into pg_restore.</sup>

To see a list of all the available options use `pg_dump -?`.

With given options `pg_dump` will first prompt for a password for the database user `db_user` and then connect as that user to the database named `db_name`. After it successfully connects, `> ` will write the output produced by pg_dump to a file with a given name, in this case `dump_name.tar`.

File created in the described process contains all the SQL queries that are required in order to replicate your database.


##Importing a PostgreSQL database dump

There are two ways to restore a PostgreSQL database:

1. `psql` for restoring from a plain SQL script file created with `pg_dump`, 
2. `pg_restore` for restoring from a .tar file, directory, or custom format created with `pg_dump`. 

###1. Restoring a a database with psql

If your backup is a plain-text file containing SQL script, then you can restore your database by using [PostgreSQL interactive terminal](https://www.postgresql.org/docs/current/static/app-psql.html), and running the following command:

```
psql -U db_user db_name < dump_name.sql
```
where `db_user` is the database user, `db_name` is the database name, and `dump_name.sql` is the name of your backup file.

###2. Restoring a a database with pg_restore

If you choose custom, directory, or archive format when creating a backup file, then you will need to use pg_restore in order to restore your database:

`pg_restore -d db_name /path/to/your/file/dump_name.tar -c -U db_user`

If you use pg_restore you have various options available, for example:

- `-c` to drop database objects before recreating them,
- `-C` to create a database before restoring into it,
- `-e` exit if an error has encountered,
- `-F format` to specify the format of the archive.

Use `pg_restore -?` to get the full list of available options.

You can find more info on using mentioned tools by running `man pg_dump`, `man psql` and `man pg_restore`.