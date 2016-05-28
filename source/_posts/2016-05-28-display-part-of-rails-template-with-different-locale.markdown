---
layout: post
title: "Display part of Rails template with different locale"
date: 2016-05-28 10:06:00 +0200
comments: true
author: Ivica Lakatoš
categories: 
  - Ruby and Rails
tags: 
  - ruby
  - rails
  - I18n.locale
published: true
---

I recently worked on a Rails project, which had parts of pages in different languages. That may be a problem if you have already translated their entire text to all required languages. You can even be tempted to hardcode parts of the text into other languages. Fortunately, there is an elegant way to solve that problem, just wrap parts of template or partials into blocks with different locale, like this:

``` ruby
<% I18n.with_locale('en') do %>
  ...part of your template
  or
  <%= render partial: 'some/partial' %>
<% end %>
```

### Example

Suppose, there is a template with only header and two paragraphs.

``` 
<h1><%= t('my_great_header') %></h1>

<p><%= t('first_paragraph') %></p>

<p><%= t('second_paragraph') %></p>
```
And locale on English and French for that template.

```
# in config/locales/en.yml
en:
  my_great_header: "My English great header"
  first_paragraph: "First English paragraph"
  second_paragraph: "Second English paragraph"
  
# in config/locales/fr.yml
fr:
  my_great_header: "My French great header"
  first_paragraph: "First French paragraph"
  second_paragraph: "Second French paragraph"
```

<!--more-->

And client want first paragraph to be always on English.

Just wrap first paragraph in block with locale `'en`, like this:

```
<h1><%= t('my_great_header') %></h1>

<% I18n.with_locale('en') do %>
  <p><%= t('first_paragraph') %></p>
<% end %>
<p><%= t('second_paragraph') %></p>
```
and when you switch language to Franch result will be:

``` text
My French great header

First English paragraph

Second French paragraph
```

I hope that this helps. Have a nice day. 