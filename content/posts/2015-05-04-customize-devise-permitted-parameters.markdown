---
layouts: posts
title: "Customize Devise permitted parameters"
date: 2015-05-04 16:07:22 +0200
comments: true
author: Jovica Šuša
categories:
  - Ruby and Rails
tags:
  - ruby
  - rails
  - devise
published: true
---
If you are using Devise gem for authentication and you have been adding custom fields to your model you’ll get in trouble when you try to create a new instance or update an existing one. All your added fields will be treated as unpermitted. The solution for this problem is to customise Devise’s configure\_permited\_parameters action. All you need to do is to add this action to your Application controller and push parameters that need to be permitted to devise\_paremeter\_sanitizer array. So let’s say you have a User Model and you have added company\_name and website fields to your user’s table, to permit this parameters on sign\_up you need to add this to your Application controller:

```ruby
def configure_permitted_parameters
  devise_parameter_sanitizer.for(:sign_up).push(:company_name, :website)
end
```

It is the same principle for the :sign\_in and :edit\_account. You can see what are [default permitted parameters here](https://github.com/plataformatec/devise/blob/master/lib/devise/parameter_sanitizer.rb#L83).
