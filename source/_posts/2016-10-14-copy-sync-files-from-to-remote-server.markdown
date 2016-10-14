---
layout: post
title: "Copy and sync files from/to remote server"
date: 2016-10-17 12:55:52 +0200
comments: true
author: Slobodan Kovačević
categories: 
  - Ruby and Rails
tags: 
  - ruby
  - rails
published: true
---

Most modern web app deployments have automated scripts that perform all tasks needed to deploy the app. They handle all the dirty details, while the developer just needs to do something simple like `cap deploy`. In other words, usually you don't need to access the remote servers directly.

However, sometimes you run into one-time tasks (or less frequent tasks) that might not have been automated. For example, dumping production data and importing on local machine, syncing uploaded files between production and staging environments, etc.

These often involve transferring files between your local machine and remote server (or two remote servers). There are few ways you can handle this depending on what you need to transfer between servers. We are going to cover methods using `wget`, `scp`, and `rsync`.
<!--more-->

## wget

Simplest option is to install `wget` on destination machine. `wget` is the non-interactive network downloader and you just give it the URL of the file you want to download.

`wget http://example.com/some/archive.tar.gz`

That would download the file to current directory.

The downside is that you have to put the file somewhere accessible via web, like `public` dir in Rails apps and also you should remember to remove it once you are done with it.

Also this only works if you have a single file, or you are able to create a single file (most likely by [creating an archive using tar and gzip](/posts/create-and-extract-archives-using-tar-and-gzip/)).

## scp

`scp` is a remote file copy program and the name is short for __s__ecure __c__o__p__y. It's very similar to usual `cp` command with the difference that it's able to copy files across different computers using SSH.

Simplest forms of `scp` have source and destination, both of which can be either a local file or a remote file.

```sh
# copy file from server to current dir on local machine
scp myuser@example.com:/home/myuser/databasedump.sql ./

# copy file to remote server
scp ./some/localfile.txt myuser@example.com:/home/myuser/
```

In a sense it works exactly like `cp`. Difference is that when specifying a remote file the format looks like you concatenated SSH user@server string and normal file path (you are saying "as user at server get this file").

There are some additional nice things about `scp`:

- you can use `-r` option which will recursively copy entire directories.
- you can specify two remote files and it will copy them between remote servers.

## rsync

`scp` is secure version of `rcp` (remote copy) program. `rsync` is faster, flexible replacement for `rcp`. It copies files either to or from a remote host, or locally on the current host (it does not support copying files between two remote hosts).

There are many ways you can use `rsync`. The most usual variants are:

```sh
# copy all files recursively from one local dir to another
rsync ./source_dir ./destination_dir

# copy a file from local dir to remote server
rsync -Pavz ./archive.tar.gz myuser@example.com:/home/myuser/somedata/


# copy all files recursively from remote server to local dir
rsync -Pavz myuser@example.com:/home/myuser/somedata ./data

```

Options that were used are not strictly necessary for doing stated tasks, but they help. They are as follows:

- `P` - same as --partial --progress. It means it will show transfer progress and if transfer breaks keep a partial copy to possibly continue on retry. Very useful for large files.
- `a` - it is a quick way of saying you want recursion and want to preserve almost everything. This is equivalent to -rlptgoD.
- `v` - be verbose.
- `z` - compress file data during the transfer.

I suggest that you consult `man rsync` for more details.
