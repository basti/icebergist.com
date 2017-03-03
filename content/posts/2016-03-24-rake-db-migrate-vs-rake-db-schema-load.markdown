---
layouts: posts
title: "rake db:schema:load vs rake db:migrate"
date: 2016-03-24 08:09:25 +0100
comments: true
author: Ivica Lakatoš
categories:
  - Ruby and Rails
tags:
  - ruby
  - rails
  - rake task
  - globalize
published: true
has_divider: true
updated_date:
---

Sooner or later every new Ruby developer needs to understand  differences between this two common rake tasks. Basically, these simple definition tells us everything we need to know:

+ `rake db:migrate` runs migrations that have not run yet
+ `rake db:schema:load` loads the schema.db file into database.

but the real question is when to use one or the other.

**Advice:** <a id="advice"></a> when you are adding a new migration to an existing app then you need to run `rake db:migrate`, but when you join to existing application (_especially some old application_), or when you drop your applications database and you need to create it again, always run `rake db:schema:load` to load schema.

### Example

I am working on application which use [globalize gem](https://github.com/globalize/globalize) for ActiveRecord model/data translations. Globalize work this way:

+ first specify attributes which need to be translatable

```ruby
class Post < ActiveRecord::Base
  translates :title, :text
end
```
<!--more-->

+ then create translation tables

```ruby
class CreatePosts < ActiveRecord::Migration
  def up
    create_table :posts do |t|
      t.timestamps
    end
    Post.create_translation_table! title: :string, text: :text
  end
  def down
    drop_table :posts
    Post.drop_translation_table!
  end
end
```
**Note** _that the ActiveRecord model Post must already exist and have listed attributes for translations_.

+ and run `rake db:migrate` .

Problem comes when you change your mind and decide to leave title to be untranslatable.

+ remove title from post translations table

```ruby
class RemoveTitleFromPostTranslations < ActiveRecord::Migration
  def up
    remove_column :post_translations, :title, :string
  end

  def down
    Entry.add_translation_fields! title: :string
  end
end
```
+ add title to posts table

```ruby
class AddTitleToPosts < ActiveRecord::Migration
  def change
    add_column :posts, :title, :string
  end
end
```
+ remove title attribute from model translations

```ruby
class Post < ActiveRecord::Base
  translates :text
end
```
+ and run `rake db:migrate`.

Everything looking good, so where is the problem?

Here it is! If you decide to delete your database and create it again you need to use:

+ `rake db:drop`
+ `rake db:create`
+ `rake db:schema:load`

Because, if you try to use `rake db:migrate` instead of `rake db:schema:load` you will get **BIG ERROR!**, because for your first migration "create_posts" it is necessary that you have defined translatable attributes :title and :text in Post model, but you removed :title from Post model translations.

So just follow [advice](#advice) above, and good luck.
