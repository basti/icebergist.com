---
date: 2008-09-17 09:00:00 +0100
title: RESTful admin namespaced controller using scaffolding
author: Slobodan Kovačević
layouts: posts
url: "/posts/restful-admin-namespaced-controller-using-scaffolding"
comments: true
categories:
  - Ruby and Rails
tags:
  - rails
  - resources
  - rest
  - restful
  - ruby
  - scaffold
published: true
---
Most of my clients prefer to have a separate admin section. In turn, I like to have separate controllers for admin section and front-end in my Rails app. This is not as straightforward as it might seem, especially if you like to use scaffolding for admin controller.

The goal is to get 2 separate RESTful controllers, admin & front-end controller, one model and for admin pages to have scaffolding.

Here is the easiest way I found so far to accomplish this. This example generates categories model and controllers for it.

`./script/generate controller admin/categories<br />
./script/generate scaffold category name:string`

This will generate an empty controller in admin namespace and a scaffolded resource for front-end controller.

Now we have everything generated we just need to make it work with admin controller and not with front-end.

*   move all templates from app/views/categories to app/views/**admin**/categories
*   copy all functions from categories\_controller.rb to admin/categories\_controller.rb
*   add namespace for admin controller in routes.rb:`map.namespace :admin do |admin|<br />
admin.resources :categories<br />
end`
*   in admin/categories\_controller.rb replace in 3 places redirect\_to calls to work with admin namespace. It will have something like redirect\_to(@category), but to work with namespace it needs to have redirect\_to([:admin, @category])
*   make similar changes in all templates, i.e. make it work within an admin namespace. You need to make following changes:
    *   form_for(@category) => **form_for([:admin, @category])**
    *   <%= link_to &#8216;Show&#8217;, @category %> => **<%= link_to &#8216;Show&#8217;, [:admin, @category] %>**
    *   categories_path => **admin\_categories\_path**
    *   edit\_category\_path(@category) => **edit\_admin\_category_path(@category)**
    *   new\_category\_path => **new\_admin\_category_path**

That&#8217;s it. Now you&#8217;ll have /admin/categories for all administrative tasks and you have a free controller for front-end actions.

You might wonder why not just generate scaffold for admin/categories&#8230; The reason is that you&#8217;ll also get a model that is namespaced in admin (i.e. model would be Admin::Category). Scaffolded views also wouldn&#8217;t work as it seems that generator doesn&#8217;t take into account the fact that you are using a namespace.
