---
redirect_to: https://www.axiomq.com/blog/how-to-skip-devise-trackable-updates/
layout: post
title: "How to skip Devise trackable updates"
date: 2014-05-12 19:10:06 +0200
comments: true
author: Slobodan Kovačević
categories:
  - Ruby and Rails
tags:
  - ruby
  - rails
  - devise
  - howto
  - tips
---

Devise has a very useful Trackable module used to track user's sign in count, timestamps and IP address. There are some occasions when you need to disable tracking. For example for API requests where user signs in on every request; for instances where admin might sign in as an user; and similar.

To disable Devise Trackable module you need to set `request.env["devise.skip_trackable"] = true`. You have to do that before trying to authenticate user, so you'll want to put it in a before_filter, or even better prepend_before_filter to make sure it's before authentication.

Add this to your controller in which you want to disable tracking:

```
prepend_before_filter :disable_devise_trackable

protected
  def disable_devise_trackable
    request.env["devise.skip_trackable"] = true
  end
```
