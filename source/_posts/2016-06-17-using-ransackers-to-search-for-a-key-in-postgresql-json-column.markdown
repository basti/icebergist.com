---
layout: post
title: "Using Ransackers to search for a key in PostgreSQL JSON column"
date: 2016-06-17 12:39:42 +0200
comments: true
author: Jovana Dačić
categories: 
  - Ruby and Rails
tags: 
  - ruby
  - rails
  - postgreSQL
  - ransack
published: true
---

Using Ransackers to search for a key in PostgreSQL JSON column
======

Starting with v9.2, PostgreSQL added native JSON support witch enabled us to take advantage of some benefits that come with NoSQL database within a traditional relational database such as PostgreSQL.

While working on a Ruby on Rails application that used PostgreSQL database to store data, we came a across an issue where we needed to implement a search by key within a JSON column.

We were alredy using [Ransack](https://github.com/activerecord-hackery/ransack) for building search forms within the application, so we needed a way of telling Ransack to perform a search by given key in our JSON column. 

This is where [Ransackers](https://github.com/activerecord-hackery/ransack/wiki/using-ransackers) come in. 
>The premise behind Ransack is to provide access to Arel predicate methods.

You can find more information on Arel [here](https://github.com/rails/arel)

In our case we needed to performa a search withing `transactions` table and `payload` column, looking for records containing a key called `invoice_number`. To acheave this we added the following ransacker to our `Transaction` model

```ruby
ransacker :invoice_number do |parent|
   Arel::Nodes::InfixOperation.new('->>', parent.table[:payload], 'invoice_number')
end
```
Now with our search set on `link_type_cont` (cont being just one of Ransack available search predicates), if the user entered for example  `123` in the search filed, it would generate a query like this:

```SQL
SELECT  "transactions".* FROM "transactions"  WHERE ("transactions"."payload" ->> 'invoice_number' ILIKE '%123%')
```

basically performing a search for records in `transactions` table that have key called `invoice_number` within a `payload` column with value containing a string `123`.