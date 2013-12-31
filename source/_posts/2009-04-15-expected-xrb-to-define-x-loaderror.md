---
title: Expected x.rb to define X (LoadError)
author: Slobodan Kovačević
layout: post
permalink: /posts/expected-xrb-to-define-x-loaderror
categories:
  - Ruby and Rails
---
I have been working on extending Rails&#8217; I18n Simple backend to make it work with Serbian grammar (post on that will follow soon), but I kept getting an error:

`Expected ./lib/serbian_simple.rb to define SerbianSimple (LoadError)`

I&#8217;ve just spent an hour trying to figure out why this keeps happening and I found that there&#8217;s a lot of people with similar problem.

It seems that the problem appears when Rails tries to autoload files. In my case there was a simple solution &#8211; I just added require &#8216;serbian_simple.rb&#8217; in environment.rb to manually load the file.
