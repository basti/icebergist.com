---
date: 2008-09-17 09:00:00 +0100
title: Mosso hosting cloud
author: Slobodan Kovačević
layout: post
permalink: /posts/mosso-hosting-cloud
comments: true
categories:
  - Misc
  - Ruby and Rails
tags:
  - cloud computing
  - hosting
  - rails
---
<a class="external" href="http://www.mosso.com/">Mosso&#8217;s hosting cloud</a>
at $100 / month seems like a good solution to get a scalable server. However one thing bugs me, actually two things&#8230;

First one: they offer **FTP only access**. Meaning you cannot deploy sites directly from code repositories (i.e. git or svn). That sucks.

Second thing that bugs me: for $100 you get quite a lot of computing power which can be used to run multiple sites &#8211; but **you are only allowed to have one Rails app running**. Only one. If you want additional Rails apps (for example to have a test server) you need to pay an extra fee.

I know it&#8217;s cloud computing and that you have to be able to run it with any additional configuration (that&#8217;s why I think it&#8217;s ok that you have to freeze your gems in Rails apps, because you cannot install any gems yourself)&#8230; but not being able to checkout my code from repository and having to upload the whole app each time you make changes **is really annoying**.

In their defense, the support guy said that they are working on it, but he could give me an ETA when they&#8217;ll allow something like that.
