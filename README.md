# Setup for blog.orangeiceberg.com

## Requirements:

* [Hugo](https://gohugo.io/)

On OS X, if you have [Homebrew](http://brew.sh/), just run:

```
$ brew update && brew install hugo
```

Clone repository

```
$ git clone git@github.com:orangeiceberg/icebergist.com.git
$ cd icebergist.com
$ mkdir static data layouts
```
## Start server

```
$ hugo server
```
You can find your site on [http://localhost:1313/](http://localhost:1313/)

## Add content

If you want to add new post eg.(year-month-day-awesome-ruby-post.md) fallow this steps:

```
$ hugo new posts/2017-03-10-awesome-ruby-post.md
```
In this step you create Markdown file inside `content/posts/2017-03-10-awesome-ruby-post.md`,
edit front matter inside `2017-03-10-awesome-ruby-post.md`

```
---
author: "FirstName LastName"
date: 2017-03-10T11:45:17+01:00
title = "Awesome Ruby post"
categories:
  - Ruby and Rails
tags:
  - ruby
  - rails
comments: true
layouts: posts
published: false # set to true when you want see changes
has_divider: false # set to true when you want to use text divider
updated_date: null # set new date when you change something important on alredy published post
---

Your text goes here.
```

## Deploying changes

I will add notes for this section later.

