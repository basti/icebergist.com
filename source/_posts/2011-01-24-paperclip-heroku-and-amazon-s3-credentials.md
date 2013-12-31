---
title: Paperclip, Heroku and Amazon S3 credentials
author: Slobodan Kovačević
layout: post
permalink: /posts/paperclip-heroku-and-amazon-s3-credentials
categories:
  - Ruby and Rails
tags:
  - amazon s3
  - heroku
  - paperclip
  - rails
  - rails3
  - ruby
---
Setting up Paperclip to use Amazon&#8217;s S3 is as simple as setting :storage => :s3 and providing right credentials to Paperclip by setting :s3_credentials option. Best way to provide S3 credentials is to use an YML file (usually config/s3.yml) which allows you to set different credentials for each environment. For example:

<pre># config/s3.yml
development:
  access_key_id: XYZXYZXYZ
  secret_access_key: XYZXYZXYZ
  bucket: mygreatapp-development
production:
  access_key_id: XYZXYZXYZ
  secret_access_key: XYZXYZXYZ
  bucket: mygreatapp-production
</pre>

Of course you want to treat s3.yml same as database.yml &#8211; i.e. you don&#8217;t want to track it with git and you want for each person/server to have it&#8217;s own.

However, consider this: you are working on Open Source app in a public git repository and you are deploying it on Heroku. Heroku doesn&#8217;t allow you to create files (unless they are in git repository) and you can&#8217;t commit s3.yml with your credentials to public repository.

One solution is to define different :s3_credentials hash in one of the environment files or to load different YML file for each environment and generate hash from it. Downside is that you need to have a separate YML file for each environment and/or you need to convert YML to hash. Other solution could be to have separate local branch from which you will push to Heroku. Problem with this is that you have to have a local branch for deploying. This means if there are multiple developers who deploy to production each should have separate local branch.

Much simpler way to deploy Paperclip with different S3 credentials for each environment (with one of the environment being deployed on Heroku; and repository being public) is to create s3.yml file as usual (and don&#8217;t commit it to git), but define values only for local environment.

For production deployment on Heroku you can write initializer which will set :s3_credentials from ENV variables.

<pre># initializers/s3.rb
if Rails.env == "production"
  # set credentials from ENV hash
  S3_CREDENTIALS = { :access_key_id => ENV['S3_KEY'], :secret_access_key => ENV['S3_SECRET'], :bucket => "sharedearth-production"}
else
  # get credentials from YML file
  S3_CREDENTIALS = Rails.root.join("config/s3.yml")
end

# in your model
has_attached_file :photo, :storage => :s3, :s3_credentials => S3_CREDENTIALS
</pre>

and you can easily set persistant ENV vars on Heroku with:

<pre>$ heroku config:add S3_KEY=XYZXYZ S3_SECRET=XYZXYZ
</pre>

([according to Heroku docs][1])

 [1]: http://docs.heroku.com/config-vars#quick-example