---
redirect_to: https://www.axiomq.com/blog/creating-and-publishing-a-ruby-gem/
layout: post
title: "Creating and publishing a ruby gem"
date: 2015-06-08 09:12:04 +0200
comments: true
author: Jovana Dačić
categories:
  - Ruby and Rails
tags:
  - ruby
  - rails
published: true
---

A gem is a simple way to distribute functionality, it can be a small plugin, a Ruby library or sometimes a whole program. Thanks to RubyGems, a gem hosting service, developers have a wide range of gems at their disposal allowing them to easily add functionality to their applications.

But what if there is no gem available that will suit the functionality you need, and you find yourself writing the same code over and over again for different projects? Well, in that case you should consider making your own gem.

It's considered a good practice to extract a gem out of an existing application, since that way you will have a better understanding of all the requirements as well as how the gem will be used. This blog post will illustrate just that on a real life example, and will take you through the process of creating a slug_converter gem.
<!--more-->
### Slug converter gem

Source code for slug_converter gem was developed while working on a link shortener application, in order to generate a string consisting of predefined characters, based on a given id number of a link. As it will be described in this blog post, this code was easily extracted from the application into an independent gem that was released on  RubyGems.

Although it may seem like a complex task at first, creating a gem is not that difficult, if you have RubyGems and Bundler installed you are good to go. We already know what RubyGems is, and Bundler is a package manager that determines a full set of direct dependencies needed by your application.

Now let's build a gem!


### Creating a gem

First step is to make sure that bundler gem is installed.
```sh
$ gem install bundler
```

once bundler is installed creating a structure for your new gem is easy,
```sh
	$ bundle gem slug_converter
```

The first time you use bundler to create a gem you will be prompted to answer a couple of questions:
```sh
	Do you want to include code of conduct in your gems you generate?
	Do you want to licence your code permissively under the MIT licence?
	Do you want to generate tests with your gem?
	Type rspec or minitest to generate those tests files now and in the future:
```

Answering these questions will help bundler configure and setup files that are being generated now and in the future. Here we answered yes to first 4 qestions and choose rspec for testing.

Running `$ bundle gem slug_converter` command resulted with "slug_converter" directory with essential gem file structure being created, and git repository initialized, assuming that you are using git for version management (as you should).
```sh
Creating gem 'slug_converter'...
 create  slug_converter/Gemfile
 create  slug_converter/.gitignore
 create  slug_converter/lib/slug_converter.rb
 create  slug_converter/lib/slug_converter/version.rb
 create  slug_converter/slug_converter.gemspec
 create  slug_converter/Rakefile
 create  slug_converter/README.md
 create  slug_converter/bin/console
 create  slug_converter/bin/setup
 create  slug_converter/LICENSE.txt
 create  slug_converter/.travis.yml
 create  slug_converter/.rspec
 create  slug_converter/spec/spec_helper.rb
 create  slug_converter/spec/slug_converter_spec.rb
```

Let's go through files that bundler generated for us, .gemspec file is the "heart" of your gem so lets start with `slug_converter.gemspec`

```ruby
lib = File.expand_path('../lib', __FILE__)
$LOAD_PATH.unshift(lib) unless $LOAD_PATH.include?(lib)
require 'slug_converter/version'

Gem::Specification.new do |spec|
  spec.name         = "slug_converter"
  spec.version      = SlugConverter::VERSION
  spec.authors      = ["Your Name"]
  spec.email        = ["youremail@example.com"]

  # if spec.respond_to?(:metadata)
  #   spec.metadata['allowed_push_host'] = "TODO: Set to 'http://mygemserver.com' to prevent pushes to rubygems.org, or delete to allow pushes to any server."
  # end

  spec.summary      = %q{Number <-> Slug converter}
  spec.description  = %q{Generates a slug based on the given number and the other way around}
  spec.homepage     = "https://github.com/orangeiceberg/slug_converter"
  spec.license      = "MIT"

  spec.files        = `git ls-files -z`.split("\x0").reject { |f| f.match(%r{^(test|spec|features)/}) }
  spec.bindir       = "exe"
  spec.executables  = spec.files.grep(%r{^exe/}) { |f| File.basename(f) }
  spec.require_paths= ["lib"]

  spec.add_development_dependency "bundler", "~> 1.8"
  spec.add_development_dependency "rake", "~> 10.0"
end
```
This file contains metadata about your gem and it can be populated directly, so here you can enter all the data such as name, description, licence... This file also contains information about what files should be packaged in your gem, as well as the load path to include the gem directory when the gem is first loaded. Most of these default settings will work for the majority of gems but you can always edit them if you want different behavior. At the bottom of the file add any gem dependencies that are required.

The version number of the gem is set in `SlugConverter::VERSION` constant which is kept in a separate version.rb file, and you can change it there for every new version of your gem.

	lib
	 |--slug_converter
			 |--version.rb

A very important part of every gem is the `README` file, where you can describe how to install and use the gem, and the `LICENCE` file where you can define the terms and conditions under which the gem can be used.

In the lib directory there is a file which has the same name as your gem (recommended), and that file will be loaded when someone requires your gem. If the gem you are writing is simple all the code can be placed in this single file, or in case of more complex gems all the other files from the lib directory are required in this file.

There is also a `Gemfile` generated, but this file doesn't have to be managed directly since all it does is look in `.gemspec` for required dependencies and then loads them through bundler. All the dependencies required by the gem should be specified in the `.gemspec` file.

Another file that is generated by the bundler is `Rakefile` which just adds some gem tasks from bundler, and we can see those tasks with explanation by running
```sh
	rake -T
	rake build    # Build slug_converter-0.0.1.gem into the pkg directory
	rake install  # Build and install slug_converter-0.0.1.gem into system gems
	rake release  # Create tag v0.0.1 and build and push slug_converter-0.1.0.gem to Rubygems
```

### Writing tests

If you are following the principles of Test Driven Development you will probably like to start by writing tests for you gem, for that purpose I would suggest using RSpec.
To do that you will need to add rspec as a development dependency to you gemspec file:
	spec.add_development_dependency 'rspec'

As mentioned in the beginning, when running bundle gem for the first time, bundler will asks if you would like to generate test files for your gem and to choose if you want to use rspec or minitest. If you answer with yes, and choose rspec, bundler will generate a spec directory with two files:

	|-- spec
    	  |-- slug_converter_spec.rb
		  |-- spec_helper.rb

In the `spec_helper.rb` file you can reference any test globals or configuration.

Since we are extracting code from an existing application we already have all the tests written so we just need to copy them into the generated `spec/slug_converter_spec.rb` file.
```ruby
require 'spec_helper'
describe SlugConverter do
  it 'has a version number' do
    expect(SlugConverter::VERSION).not_to be nil
  end
  describe ".number" do

    it "returns number when number is set" do
      converted_slug= SlugConverter.new(111)
      expect(converted_slug.number).to eq(111)
    end

    it "returns decoded number for existing slug" do
      converted_slug = SlugConverter.new("vg")
      expect(converted_slug.number).to eq(363)
    end

  end

  describe ".number" do

    it "sets number to given value" do
      converted_slug = SlugConverter.new(211)
      expect(converted_slug.number=210).to eq(210)
    end

    it "sets slug to encoded value of number" do
      converted_slug = SlugConverter.new(211)
      converted_slug.number=210
      expect(converted_slug.slug).to eq("pb")
    end

    it "sets number to integer value of given number passed as string" do
      converted_slug = SlugConverter.new("210")
      expect(converted_slug.number).to eq(210)
    end

    it "sets slug to encoded value of given number passed as string" do
      converted_slug = SlugConverter.new("210")
      expect(converted_slug.slug).to eq("pb")
    end

    it "sets number to integer value of argument that starts with a number but also contains letters" do
      converted_slug = SlugConverter.new("210jj")
      expect(converted_slug.number).to eq(210)
    end

    it "sets slug to encoded value of argument that starts with a number but also contains letters" do
      converted_slug = SlugConverter.new("210jj")
      expect(converted_slug.slug).to eq("pb")
    end

  end

  describe ".slug" do

     it "returns slug when slug is set" do
        converted_slug = SlugConverter.new("hy")
        expect(converted_slug.slug).to eq("hy")
     end

     it "returns encoded slug when link id is set" do
        converted_id = SlugConverter.new(113)
        expect(converted_id.slug).to eq("hy")
     end

  end

  describe ".slug" do

    it "sets slug to given value" do
      converted_slug = SlugConverter.new("ezk")
      expect(converted_slug.slug=("ebk")).to eq("ebk")
    end

    it "sets number to decoded value of slug" do
      converted_slug = SlugConverter.new("pb")
      converted_slug.slug=("ezk")
      expect(converted_slug.number).to eq(1483)
    end

    it "raises Arrgument Error exception if given value is an empty string" do
      expect { SlugConverter.new("") }.to raise_error(ArgumentError)
    end

    it "raises Arrgument Error exception if given value is nil" do
      expect { SlugConverter.new(nil) }.to raise_error(ArgumentError)
    end

    it "raises Arrgument Error exception if given value contains unpermitted letters" do
      expect { SlugConverter.new("iiii") }.to raise_error(ArgumentError)
    end

    it "raises Arrgument Error exception if given value starts with letter but contains numbers" do
      expect { SlugConverter.new("bb12") }.to raise_error(ArgumentError)
    end

  end
end
```

To make rspec rake task available we will setup tasks folder where we'll place our `rspec.rake` file containing only 2 lines:
```ruby
	require 'rspec/core/rake_task'
	RSpec::Core::RakeTask.new(:spec)
```

and then we will import this file in our Rakefile that bundler provided automatically:
```ruby
	Dir.glob('tasks/**/*.rake').each(&method(:import))
```

Now run:
```sh
bundle exec rake spec
```
And watch your tests fail. :)


### Add gem functionality

Now we need to make those test go green. To do that we will again copy the existing code from our application in the main gem file `lib/slug_converter.rb`:
```ruby
require "slug_converter/version"
require 'set'
require 'gem_config'

class SlugConverter
  include GemConfig::Base
  with_configuration do
    has :alphabet, default: "qjeghxtrpnfmdzwvsybkuoca"
  end

  def initialize(number_or_slug)
     @alphabet = SlugConverter.configuration.alphabet.split(//)
    if number_or_slug.to_i != 0
      @number = number_or_slug.to_i
    elsif validate_string(number_or_slug)
      @slug = number_or_slug.downcase
    else
      raise ArgumentError, 'Argument must be integer value or non-empty string consisting of predefined letters'
    end
  end

  def number
    if @number.nil?
      @number = bijective_decode
    else
      @number
    end
  end

  def number=(new_number)
    @number = new_number
    @slug = bijective_encode
    @number
  end

  def slug
    if @slug.nil?
      @slug = bijective_encode
    else
      @slug
    end
  end

  def slug=(new_slug)
    @slug = new_slug
    @number = bijective_decode
    @slug
  end

  private

    def bijective_encode
      id = @number
      return @alphabet[0] if id == 0
      s = ''
      base = @alphabet.length
      while id > 0
        s << @alphabet[id.modulo(base)]
        id /= base
      end
      s.reverse
    end

    def bijective_decode
      i = 0
      base = @alphabet.length
      @slug.each_char { |c| i = i * base + @alphabet.index(c) }
      i
    end

    def validate_string(slug)
      unless slug.nil?
        alphabet = Set.new @alphabet
        slug_letters = Set.new slug.downcase().split(//)
        slug != "" && (slug_letters.subset? alphabet)
      end
    end
end
```

Now when we run the tests again, they should all pass.

###Making your gem configurabile

In order to allow users to set their own alphabet that will be used by the SlugConverter, we needed to make our gem configurabile. To do this we used https://github.com/krautcomputing/gem_config gem.

You will notice this code at the begining of the SlugConverter class:
```ruby
class SlugConverter
  include GemConfig::Base

  with_configuration do
    has :alphabet, default: "qjeghxtrpnfmdzwvsybkuoca"
  end

  def initialize(number_or_slug)
     @alphabet = SlugConverter.configuration.alphabet.split(//)
     # ...
  end	  

  # rest of the code omitted    
end
```
this code along with `spec.add_runtime_dependency 'gem_config'` added as a dependency in `slug_converter.gemspec` file, alows us to make the gem configureabile.

Custom aphabet can than be defined by adding `config/initializers/slug_converter.rb` to your application, and defining the alphabet like this:
```ruby
SlugConverter.configuration.alphabet = "your_custom_alphabet_here"
```



### Releasing your gem


Now that we have the test passing and all the code in place it's time to make the gem available for everyone to use by releasing it on RubyGems, to do that you will need to have a RubyGems account. If this is the first time you release a gem you will be prompted to enter your RubyGems username and password. You will also need to have your repository setup on Github.

Then with just one comand:
```sh
	$ bundle exec rake release
```

* your code will be pushed to your Github repository,
* your git repository will be tagged with the version number using a name like "v1.0.0".
* your gem released on RubyGems.

The ruby gem described in this blog post can be found here https://rubygems.org/gems/slug_converter, and all the code is in this GitHub repository https://github.com/orangeiceberg/slug_converter .
