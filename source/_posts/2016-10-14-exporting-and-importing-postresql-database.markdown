---
layout: post
title: "exporting_and_importing_postresql_database"
date: 2016-10-14 14:05:38 +0200
comments: true
author: Jovana Dačić
categories: 
  - Ruby and Rails
tags: 
  - ruby
  - rails
  - postgreSQL
published: false
---

#Backing up and Restoring a PostgreSQL database

While working on different projects and in different environments, we offten need to export a dump from one database and then import it to another. A while ago [Slobodan](http://http://orangeiceberg.com/about/ "About Slobodan") wrote how to [export and import mySQL dump](http://icebergist.com/posts/import-and-export-mysql-dump/ "Import and Export mySQL dump"), here is how to do it for postgreSQL.

##Exporting a PostgreSQL database dump

To export postreSQL database we will need to use [pg_dump](https://www.postgresql.org/docs/current/static/backup-dump.html "PostgreSQL" ) tool which will dump all the contents of a selected database into a single file. 
We need to run pg_dump in the command line on the computer where the database is stored. So if the database is stored on a remote server, you will need to SSH to that server in order to run the following command: 

```
pg_dump -U db_user -W -F t db_name > /path/to/your/file/dump_name.tar
```
Here we used options:

* `-U` to specify the user which will connect ot the PostgreSQL database server.
*  `-W` or `--password` will force pg_dump to prompt for a password before connecting to the server. 
*  `-F` - is used to specify the format of the output file, which can be one of the following:
*
	* `p` plain - plain-text SQL script
	* `c` custom 	- custom-format archive
	* `d` directory - directory-format archive
	* `t` tar - tar-format archive
	
<sup>*custom*, *directory* and *tar* formats are suitable for input into pg_restore.</sup>

With given options `pg_dump` will first prompt for a password of `db_user` postgres user and then connect as that user to database with name `db_name`, after it is connected `> ` will write the output produced by pg_dump to a file with a given name, in this case `dump_name.tar`.

File created in the decsribed process contains all the SQL queries that are required in order to replicate your database. 


##Importing a PostgreSQL database dump

There are two ways to restore a PostgreSQL database: 

1. `psql` for restoring from a plain SQL script file created with `pg_dump`
2. `pg_restore` for restoring from tar file, directory or custom format created with `pg_dump`

###1. Restoring a a database with psql

If you chose option `-F p` when creating your database backup file, then you can restore your database by using [PostgreSQL interactive terminal](https://www.postgresql.org/docs/current/static/app-psql.html) and running the following command:

```
psql -U db_user db_name < dump_name.sql
```

###2. Restoring a a database with pg_restore

If you choose any of the options `c`, `d` or `t` for format when creating your database backup file, then you will need to use `pg_restore` in order to restore your database. If you use this option to restore your database you have various options available, for example:

- `-c` to drop database objects before recreating them
- `-C` to create a database before restoring into it
- `-e` exit if an error has encountered 
- `-F format` to specify the format of the archive

you can find full list of availabe options available for `pg_restore` [here](https://www.postgresql.org/docs/current/static/app-pgrestore.html)

To restore your database from a backup file you will need to run the following command:

`pg_restore -d db_name /path/to/your/file/dump_name.tar --clean -U db_user`
 
You can find more info on using mentioned tools by running `man pg_dump`, `man psql` and `man pg_restore`.



