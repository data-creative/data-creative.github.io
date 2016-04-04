---
layout: post
title:  "How to create a Ruby on Rails application"
author: MJ Rossetti
categories:
 - posts
 - process-documentation
img: rails-logo.svg
tags: none
published: true
icon_class: none
technologies:
 - git
 - ruby
 - rubygems
 - bundler
 - rails
 - rake
 - bower
credits:
 - http://blog.blenderbox.com/2014/04/16/gitignore-everything-inside-a-directory/
 - https://www.digitalocean.com/community/tutorials/how-to-create-a-new-user-and-grant-permissions-in-mysql
---

This document describes the process of creating a new Ruby on Rails application from scratch, using Rails version 4.2.0. Replace `MY_APP` below with the name of your app, `MY_NAME` with your name, and `MY_EMAIL` with your email address.

## Generating

Create a new app and set up version control.

```` sh
bundle exec rails new MY_APP --database=mysql
cd MY_APP/
git init .
git add .
git commit -m "generating new rails app"
````

Create and edit **README.md**, and **LICENSE.md**. For the license, use this MIT template:

````
The MIT License (MIT)

Copyright (c) 2015 MY_NAME <MY_EMAIL@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
````

Commit the documentation.

```` sh
git commit -am "adding documentation"
````

## Configuring

### Managing Back-end Packages

Revise **Gemfile** to add ruby version, and [rubygems](https://rubygems.org/)
 like 'pry' for debugging, 'rspec-rails' for testing, and 'yard' for documentation.

```` rb
# Gemfile
# ...
ruby "2.2.0"
# ...
gem 'yard', group: :doc # run `bundle exec yard doc` to parse comments and/or `bundle exec yard server` to view documentation at *localhost:8808*
# ...
group :development, :test do
  # ...
  gem 'pry'
  gem 'rspec-rails', '~> 3.0'
end
````

Install gems via *bundler*:

```` sh
bundle install
````

#### Configuring Documentation

```` sh
bundle exec yard doc
````

Remove documentation from source control by adding a file at *.yardoc/.gitignore* and *doc/.gitignore* with the following contents:

    # Ignore everything in this directory
    *
    # Except this file
    !.gitignore

#### Configuring the Database

Ensure existence of the database user and password specified in **/config/database.yml**, using the command line or relational database management software as necessary.

```` sql
-- replace 'myuser' and 'mypassword' with your own custom values:
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypassword';
GRANT ALL ON *.* to 'myuser'@'localhost';
````

Create the development and test environment databases.

```` sh
bundle exec rake db:create
````

#### Configuring the Test Suite

Finish rspec installation. Refer to [rspec installation instructions](https://github.com/rspec/rspec-rails#installation) as necessary.

```` sh
bundle exec rails generate rspec:install
````

Remove tests directory.

```` sh
rm -rf test/
````

Configure generators in **config/application.rb**.

```` rb
# configure generators ...
config.generators do |g|
  g.test_framework :rspec
  g.assets = false
  g.helper = false
end
````

Run tests to ensure proper configuration.

```` sh
bundle exec rspec spec/
````

Commit the configurations.

```` sh
git commit -am "configuring tests and database"
````

### Managing Front-end Packages

Create a file called **package.json** in the root directory, optionally generating with `npm init`:

    {
      "name": "my-app",
      "version": "0.0.1",
      "dependencies": {
        "bower": "~1.4"
      },
      "engine": {
        "node": "0.12.7",
        "npm": "2.12.1"
      },
      "scripts": {
        "postinstall": "./node_modules/bower/bin/bower install"
      }
    }

Create a file called *bower.json* in the root directory, optionally generating with `bower init`:

    {
      "name": "my-app",
      "dependencies": {
        "bootstrap": "~3.3.4",
        "d3": "~3.5.5",
        "jquery": "~2.1.4",
        "jquery.ui": "~1.11.4",
        "octicons": "*"
      }
    }


Create a file called *.bowerrc* in the root directory of your app, to resemble the following template:

    {
      "directory": "vendor/assets/components"
    }

Install components with `bower install`. Alternatively run `npm install`, which, if **package.json** is configured according to the template above, will also perform a bower installation.

Require components in *app/assets/javascripts/application.js*:

    //= require d3/d3

> NOTE: place all component require statements above `//= require_tree .`

Remove installed node modules and bower components from source control by adding a file at *node_modules/.gitignore* and *vendor/assets/components/.gitignore* with the following contents:

    # Ignore everything in this directory
    *
    # Except this file
    !.gitignore

Commit the configurations.

```` sh
git commit -am "configuring front-end packages"
````

## Writing

That's it. Now start coding your custom app.

Write [rspec tests](http://betterspecs.org/) as necessary.

Write comments above public-facing methods as necessary, according to the [yard](http://yardoc.org/) documentation specification.
