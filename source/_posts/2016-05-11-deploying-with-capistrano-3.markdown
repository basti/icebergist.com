---
layout: post
title: "Deploying with capistrano 3"
date: 2016-05-20 08:42:17 +0200
comments: true
author: Ivica Lakatoš
categories:
  - Ruby and Rails
tags:
  - ruby
  - rails
  - deployment
  - capistrano 3
  - capistrano-rails
  - capistrano-rbenv
published: true
---

In the lifetime of every application the time comes for it to be presented to everyone. That's why we have to put our application on a special server which is designed for this purpose. In one word, we need to **deploy** our application. In this post you will see how to deploy app with [Capistrano 3](http://www.capistranorb.com/).  

Capistrano is a great developers tool that is used to automatically deploy projects to remote server.

### Add Capistrano to Rails app

I will assume you already have a server set up and an application ready to be deployed remotely.

We will use gem ['capistrano-rails'](https://github.com/capistrano/rails), so we need to add this gems to Gemfile:

``` ruby
group :development do
  gem 'capistrano', '~> 3.5'  
  gem 'capistrano-rails', '~> 1.1.6'
end
```
and install gems with `$ bundle install`.

### Initialize Capistrano

Then run the following command to create configuration files:

```
$ bundle exec cap install
```


This command  creates all the necessary configuration files and directory structure with two stages, staging and production:

```
Capfile
config/deploy.rb
config/deploy/production.rb
config/deploy/staging.rb
lib/capistrano/tasks 
```
<!--more-->

### Require needed gems in Capfile

Open the `Capfile` and add or uncomment this lines:

```
require 'capistrano/setup'
require 'capistrano/deploy'
require 'capistrano/bundler'
require 'capistrano/rails/assets'
require 'capistrano/rails/migrations'
Dir.glob('lib/capistrano/tasks/*.rake').each { |r| import r }
```
#### Add capistrano-rbenv gem

The [capistrano-rbenv](https://github.com/capistrano/rbenv) gem provides rbenv support for Capistrano 3.

Add this line to the Gemfile:

```
group :development do
  gem 'capistrano', '~> 3.5' 
  gem 'capistrano-rails', '~> 1.1.6'
  gem 'capistrano-rbenv', '~> 2.0', require: false
end
```

And require this gem in Capfile `require 'capistrano/rbenv'`.

#### Add capistrano-passenger gem

The [capistrano-passenger](https://github.com/capistrano/passenger) gem adds a task to restart your application after deployment via Capistrano.

```
group :development do
  ...
  gem 'capistrano-rbenv', '~> 2.0', require: false
  gem 'capistrano-passenger', '~> 0.2.0'
end
```
And require this gem in Capfile `require 'capistrano/passenger'`.

### Configure deploy.rb file

Open `config/deploy.rb` and add options for deployment:

+ set all needed variables, this is the variant with two servers (_staging and production_) and with user created on server (_server setup is theme for different post_)

```
set :application, 'app-name'   # application name
set :deploy_user, 'user-name'   # name of user who is set on server
set :repo_url, 'git@github.com:nickname/repo_name.git'   # your repository url from github 
set :branch, ENV.fetch('BRANCH', 'master')   # branch which you want to deploy from
```
+ set the path where you want to find your app on server, starting from server's root

```
set :deploy_to, -> { "/path/to/app/#{fetch(:rails_env)}-#{fetch(:application)}" }
```

+ set config files, Capistrano uses a folder called shared to manage files and directories that should persist across releases

```
set :linked_files, fetch(:linked_files, []).push('config/database.yml', 'config/secrets.yml')

set :linked_dirs, fetch(:linked_dirs, []).push('log', 'tmp/pids', 'tmp/cache', 'tmp/sockets', 'vendor/bundle', 'public/system')

```

+ set ruby version, we use `gem 'capistrano-rbenv'` for this setup

```
set :rbenv_type, :user
set :rbenv_ruby, '2.2.2'
```

+ set option for restarting your application after deployment with `gem 'capistrano-passenger'`

```
set :passenger_restart_with_touch, true
```

+ here you can put all kinds of rake tasks for different needs that you can run every time when you deploy your application.

```
namespace :deploy do
    desc "Description of task"
    task :name_of_task do
        # do something
    end
end
```

### Capistrano's server settings

You need to tell Capistrano where to find your server.
This is an example of server's settings for application where everything is on same machine (application, server, database).

+ In `config/deploy/staging` set:

```
server 'your.staging.server.com', user: fetch(:deploy_user), roles: %w{app db web}
```

+ and set rails environment

```
set :rails_env, 'staging'
```

Also set the same configuration for production server.

In `config/deploy/production` add:

```
server 'your.production.server.com', user: fetch(:deploy_user), roles: %w{app db web}
set :rails_env, 'production'
```

### Deploy your application

Just run `deploy` task:

```
bundle exec cap staging deploy
```

or

```
bundle exec cap production deploy
```
and that is it, your app is live and you can visit it on server's name url, in our example case _your.staging.server.com_.

Note: you can find complete documentation on [Capistrano site](http://capistranorb.com/).

Have a nice day!