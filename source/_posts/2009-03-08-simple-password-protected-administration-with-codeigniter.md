---
date: 2009-03-08 09:00:00 +0100
title: Simple password protected administration with CodeIgniter
author: Slobodan Kovačević
layout: post
permalink: /posts/simple-password-protected-administration-with-codeigniter
comments: true
categories:
  - PHP
tags:
  - admin
  - codeigniter
  - erkanaauth
  - howto
  - password protected
  - PHP
  - tutorial
---
Last week I&#8217;ve taken a break from Ruby/Rails development and I&#8217;ve worked on a site that uses PHP with [CodeIgniter][1] framework.

Despite the fact that CodeIgniter has a very nice documentation I found it very difficult to find a way to do some simple things, that are more or less obvious, but which can be a problem for someone who hasn&#8217;t worked with CodeIgniter before. (for example, I found myself more than once looking at CI code to figure out how it works, so I can use it)

I had to make a simple password protected administration section. One admin user, one password, no user registrations, no roles &#8211; simple as possible. As I was using CI framework I decided to find a plugin/library that does this. Unfortunately most CI authorization plugins/libraries are very bloated and too complicated for this simple task. I tried to find some examples how to handle this simple use case, but nothing came up.

Finally I&#8217;ve found a small authorization plugin: [Erkanaauth][2].

First you need a user table (must be named &#8216;users&#8217;) which only needs to have an id field and all other fields are optional because you will manually specify what other columns will be used. I opted for simple id, username, password:

<pre>CREATE TABLE IF NOT EXISTS `users` (
  `id` int(11) NOT NULL auto_increment,
  `username` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  PRIMARY KEY  (`id`)
);</pre>

We will need to &#8220;install&#8221; ErkanaAuth library. You should [download it][3] and unzip it.

Next we should create an Admin controller which will handle all administration tasks (remember I&#8217;m making simple admin here, so I don&#8217;t need to protect multiple controllers).

<pre>&lt;?php
class Admin extends Controller {
  function Admin()
  {
    parent::Controller();   
    $this->load->database();
    $this->load->helper(array('url', 'form', 'date'));
    $this->load->library(array('form_validation', 'upload', 'Erkanaauth', 'session'));
  }
}
?&gt;</pre>

Constructor just connects to database and loads some standard helpers and libraries (including Erkanaauth) that are usually used.

Next step is to add a function which we can call to verify if user is logged in:

<pre>private
  function authorize()
  {
	  if($this->erkanaauth->try_session_login())
	      return true;
  
	  redirect('admin/login');
  }
</pre>

Function uses Erkanaauth&#8217;s try\_session\_login which checks if user is already logged in (checks session for user id). If user isn&#8217;t logged in we&#8217;ll redirect him to our login page:

<pre>function login()
  {
    $username = $this->input->post('username', true);
    $password = $this->input->post('password', true);
    if($username || $password)
    {
      if($this->erkanaauth->try_login(array('username' => $username, 'password' => $password)))
        redirect('admin');
    }
    
    $this->load->view('admin_login');
  }

  function logout()
  {
    $this->erkanaauth->logout();
    redirect('admin');
  }
</pre>

Key command here is try_login in login function which tries to find an entry in users table that fulfills given conditions. If you have different users table than the one I made this is the place where you should enter your column names.

Logout function is has just a simple call to Erkana&#8217;s logout function. Nothing special there.

Of course we also need a login page template which should contain a simple user/pass form. It&#8217;s pretty basic and you can see it if you get the complete code (see at the end).

Finally we have everything needed to protect any page in Admin controller. In order to protect a page all you need to do is to add a call to authorize function to any function you want to protect. Like this:

<pre>function index()
  {
    $this->authorize();
    echo "Do something useful... For now just display logout link: ";
    echo anchor('admin/logout', "Logout");
  }
</pre>

That&#8217;s it. Now you have fully functional administration section which requires username and password authorization.

You can get the complete sample application from [Github repository][4]. Feel free to expand on it or use it any way you like.

[1]: http://codeigniter.com/
[2]: http://codeigniter.com/wiki/Erkana/
[3]: http://codeigniter.com/wiki/File:erkanaauth.zip/
[4]: http://github.com/basti/ci-admin-section/tree/master
