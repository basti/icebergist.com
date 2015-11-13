---
layout: post
title: "Create and extract archives using tar and gzip"
date: 2015-11-13 13:28:36 +0100
comments: true
author: Slobodan Kovačević
categories: 
  - Misc
tags: 
  - tips
  - howto
  - linux
published: true
---


One of the simplest tasks is creating and extracting files using `tar` and `gzip`. Yet for most new developers this is a daunting task. These days `tar` is mostly used to simply combine a few files into a single file and then `gzip` is used to compress that file.

Here is a quick overview how to use `tar` and `gzip` to create and compress an archive:

```sh
# archive individual files
tar -cvzf myarchive.tar.gz /path/to/file1 /path/to/file2

# archive whole directory
tar -cvzf myarchive.tar.gz /path/to/dir

# archive whole directory but don't store full path
tar -cvzf myarchive.tar.gz -C /path/to/dir ./
```

Options give to tar are: `c` to create new archive, `v` to be verbose, `z` to compress resulting archive with `gzip`, and `f` to write the archive to specified file. After options you can list files and dirs you want to archive.

In all examples we provide a full path to a file or dir we want to archive. In this case `tar` will store files in the archive using the full path. This means once you extract the files you'll have a complete directory structure from root dir onwards.

The way to avoid this is either to manually `cd` to dir in which files are stored, or to tell `tar` using `C` option to change dir before archiving files.

Finally to extract an archive:

```sh
tar -xvzf myarchive.tar.gz
```

The `x` option tells `tar` to extract the archive into current directory.

For more information you can consult manual using `man tar`.