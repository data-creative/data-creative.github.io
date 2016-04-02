---
layout: post
title:  "How to create and manage a Ruby gem"
author: MJ Rossetti
categories:
 - posts
 - process-documentation
img: rubygems_logo_red.png
tags: none
published: true
icon_class: none
technologies:
 - ruby
 - rubygems
 - bundler
 - rake
 - rails
credits:
  - http://guides.rubygems.org/make-your-own-gem/
  - http://rakeroutes.com/blog/lets-write-a-gem-part-one/
  - http://rakeroutes.com/blog/lets-write-a-gem-part-two/
  - http://elgalu.github.io/2013/tools-for-creating-your-first-ruby-gem/
  - http://www.alexedwards.net/blog/how-to-make-a-rubygem-part-two
  - https://gist.github.com/nesquena/1406718
---

This document describes the process of creating and managing a [Ruby gem](https://rubygems.org/).

## Generating

```` sh
bundle gem mygem
cd mygem
````

## Describing

Update *mygem.gemspec*, specifically the summary, description, and homepage attributes.

Update *README.md* to describe desired functionality,
 and include the following sections at the bottom:

    ## Contributing

    Browse existing [issues](https://github.com/ORNAME/REPONAME/issues) or create a new issue to communicate bugs, desired features, etc.

    After forking the repo and pushing your changes, create a pull request referencing the applicable issue(s).

    ### Developing

    After checking out the repo, run `bin/setup` to install dependencies.

    ### Testing

    Run `rake rspec` or `bundle exec rspec spec/` to run the tests.

    You can also run `bin/console` for an interactive prompt that will allow you to experiment.

    ### Releasing

    To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

    ## [License](LICENSE.txt)


## Writing

Make the library do what you said it would do. Revise *README.md* as necessary.

Have fun!

## Testing

The gem generation process configures much of the rspec test suite for you.

Create one or more rspec tests (*spec/myclass_spec.rb*) using the following template:

    require 'spec_helper'

    Mygem
      RSpec.describe Myclass do
        describe '#all' do
          it "does some stuff" do
            expect(true)
          end
        end
      end
    end

Run tests.

```` sh
bundle exec rspec spec/
````

## Debugging

Revise *mygem.gemspec* to include the following development dependencies:

    spec.add_development_dependency "pry", "~> 0.10"

To facilitate debugging inside tests, revise *spec/spec_helper.rb* to include:

    require 'pry'

To use pry instead of IRB as the default console, revise *bin/console* to include:

    require 'pry'
    Pry.start

Start the console.

    bin/console

> If debugging in a rails console, revise the rails application's *Gemfile* to include:
 `gem 'mygem', '~> 0.0.1', :path => '../mygem'`

## Documenting

Write comments above public-facing methods according to the [YARD](http://yardoc.org/) specification.

Revise *mygem.gemspec* to include the following development dependency:

    spec.add_development_dependency "yard"
    spec.add_development_dependency "redcarpet"
    spec.add_development_dependency "github-markup"

Add a *.yardopts* file to include the following configuration options:

    --markup-provider=redcarpet
    --markup=markdown

After installing with `bundle install`, run `bundle exec yard doc` to parse comments and/or `bundle exec yard server` to view documentation at *localhost:8808*.

## Versioning and Releasing

Edit gem version in *lib/mygem/version.rb*.

Run `bundle exec rake release` to auto-generate git tag and push to github and rubygems.
