---
title: Random Rails tips
author: Slobodan Kovačević
layout: post
permalink: /posts/random-rails-tips
categories:
  - Ruby and Rails
---
Here are some random Rails tips I&#8217;ve found useful:

*   **rails console sandbox** &#8211; if you open console like this it will rollback all database changes once you exit. Pretty useful for playing around without making any changes to database.
*   **rake db:migrate:status** &#8211; useful when you want to see the status of current database. It will show the status of all migrations.
*   **User.pluck(:email)** &#8211; since Rails 3.2.1 you can use [pluck][1] method to get an array of values in one particular column. It&#8217;s the equvivalent of doing User.select(:email).map(&:email)

 [1]: http://apidock.com/rails/ActiveRecord/Calculations/pluck