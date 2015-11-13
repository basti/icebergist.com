---
layout: post
title: "changing database starting id"
date: 2015-06-01 11:03:47 +0200
comments: true
author: Jovica Šuša
categories: 
  - Ruby and Rails
tags: 
  - ruby
  - rails
  - sqlite
  - postgres
  - mysql
published: true
---

For our new project it was necessary to modify the starting id of our database. This can be handled through migration for creating table but we decided to create a rake task that handled this for us.

The rake task that we created detects what database is being used and executes appropriate changes according to that.
<!--more-->
You can create a rake task using rails generate command for rake task:

```
rails g task namespace task_name
```
This will create your task in lib/tasks with chosen namespace and task name.

Here is our task and an explanation that follows.

```ruby
namespace :database do
  desc "Detect database that's being used and then increment its id"
  task autoincrement: :environment do
  
    db_name_downcase = ActiveRecord::Base.connection.adapter_name.downcase

    if Link.maximum(:id).to_i < 1000
      if db_name_downcase.start_with? "mysql"
        ActiveRecord::Base.connection.execute("ALTER TABLE links AUTO_INCREMENT = 1000")
      end
      if db_name_downcase.start_with? "postgres"
        ActiveRecord::Base.connection.execute("ALTER SEQUENCE links_id_seq START with 1000 RESTART;")
      end
      if db_name_downcase.start_with? "sqlite"
        ActiveRecord::Base.connection.execute("insert into sqlite_sequence(name,seq) values('links', 1000)")
      end
    else
      puts "To perform this task your database shouldn't have records with id number higher than 1000"
    end
    
  end
end
```
We need to change the starting id of our database to 1000 so we check that we don't have a record with id higher than 1000. Link is our Active Record model and links is the name of our table.

ActiveRecord::Base.connection returns the connection currently associated with the class. We use it to detect the name of database and execute appropriate changes.
###MySQL
For MySQL we need to set AUTO_INCREMENT value to 1000, Auto-increment allows a unique number to be generated when a new record is inserted into a table. When first record is created it sets its primary key to 1 by default  and it will auto increment by 1 for each new record.
###PostgreSQL
For Postgres we have to explain what a sequence is. A sequence is a special kind of a database object designed for generating unique numeric identifiers. It is typically used to generate artificial primary keys. Sequences are similar to the Auto-increment concept in MySQL.
###SQLite
For SQlite we altered sqlite_sequence table, which is an internal table used to implement AUTOINCREMENT. It is created automatically whenever any ordinary table with an AUTOINCREMENT integer primary key is created.

You can check [this Stack Overflow discussion](http://stackoverflow.com/questions/2075331/change-starting-id-number) that was very helpful to me.

    