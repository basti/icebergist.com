---
redirect_to: https://www.axiomq.com/blog/rspec-and-factorygirl-setup-for-testing-carrierwave-uploaders/
date: 2013-09-07 09:00:00 +0100
title: RSpec and FactoryGirl setup for testing Carrierwave uploaders
author: Slobodan Kovačević
layout: post
permalink: /posts/rspec-and-factorygirl-setup-for-testing-carrierwave-uploaders
comments: true
categories:
  - Ruby and Rails
  - Testing
tags:
  - carrierwave
  - factorygirl
  - factory_girl
  - rails
  - rspec
  - ruby
  - tdd
---
Assume that you have the usual setup with model (MyFile) using simple Carrierwave uploader (MyFileUploader):

<pre># app/models/my_file.rb
class MyFile &lt; ActiveRecord::Base
  mount_uploader :file, MyFileUploader
end</pre>

To be able to test Carrierwave uploaders with RSpec using FactoryGirl factories you need:

*   <span style="line-height: 13px;">define factory with uploaded file</span>
*   modify test environment storage so test file uploads are separated from other uploads
*   turn off image processing to speed up tests
*   perform cleanup after each test

## Define factory

<pre># spec/factories/my_files.rb
FactoryGirl.define do
 factory :my_file do
   photo Rack::Test::UploadedFile.new(File.open(File.join(Rails.root, '/spec/fixtures/myfiles/myfile.jpg')))
 end
end</pre>

## Setup Carrierwave

First we need to make sure Carrierwave is using local file system for storage and to disable file processing for testing environments. Disabling file processing will speed up tests considerably. We can do that by adding following to Carrierwave initializer:

<pre>if Rails.env.test? || Rails.env.cucumber?
  CarrierWave.configure do |config|
    config.storage = :file
    config.enable_processing = false
  end
end</pre>

Next we should separate test uploads from any other uploads. We can do that by modifying cache\_dir and store\_dir methods for all Carrierwave models (i.e. all models that are descendants of CarrierWave::Uploader::Base). So the whole Carrierwave initializer looks something like:

<pre># config/initializers/carrierwave.rb
if Rails.env.test? || Rails.env.cucumber?
  CarrierWave.configure do |config|
    config.storage = :file
    config.enable_processing = false
  end

  # make sure our uploader is auto-loaded
  MyFileUploader

  # use different dirs when testing
  CarrierWave::Uploader::Base.descendants.each do |klass|
    next if klass.anonymous?
    klass.class_eval do
      def cache_dir
        "#{Rails.root}/spec/support/uploads/tmp"
      end

      def store_dir
        "#{Rails.root}/spec/support/uploads/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}"
      end
    end
  end
end</pre>

## Clean up uploaded files

Using factory defined above will create uploaded files in cache\_dir and store\_dir. These are just temporary files and should be removed after each test, so each of them has a clean slate. By adding after :each hook in RSpec configuration block we can remove these files simply by deleting spec/support/uploads dir.

<pre># spec_helper.rb
RSpec.configure do |config|
  config.after(:each) do
    if Rails.env.test? || Rails.env.cucumber?
      FileUtils.rm_rf(Dir["#{Rails.root}/spec/support/uploads"])
    end
  end
end</pre>
