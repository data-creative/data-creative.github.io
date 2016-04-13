---
layout: post
title:  "Node.js for Rails developers, Part 2 (Node and Express)"
author: MJ Rossetti
published: true
img: nodejs-logo-green.png
#repo_url: ______________
#project_url: https://express-robots.herokuapp.com/
categories:
 - process-documentation
technologies:
 - git
 - node.js
 - npm
 - express.js
---

This post is part of a series for *Rails* developers who want to get started with *Node.js*. After [discussing technology options](/process-documentation/2016/04/09/node-for-rails-developers-part-1-choose-stack/), we're ready to get started with the essential technologies - *Node* and *Express*. The goal of this post is to generate a new application and view it in a local web browser.

<hr>

## Installing Node

Unless you've already installed *Node* on your computer, use *Homebrew* to install it now.

```` sh
brew install node
````

This command also installs *NPM*, the primary package manager for *Node*. For *Rails* developers, *NPM* is like *Bundler*.


## Installing Express

Use *NPM* to install the [*Express Generator*](http://expressjs.com/en/starter/generator.html) module. This package includes *Express* and a command-line utility for generating new *Express* applications.

Install *Express Generator* globally.

```` sh
npm install express-generator -g
````

> NOTE: Passing the `-g` flag denotes a global installation. Global installations generally allow the module to be invoked from the command line.

## Generating a New Express Application

Use the *Express Generator* to generate a skeleton directory structure for a new *Express* app.

```` sh
express robots_app --ejs
````

> NOTE: The `--ejs` flag specifies our choice to use *EJS* as a view template engine instead of the default template engine, *Jade*.

This command should create the following files:

    robots_app
    robots_app/package.json
    robots_app/app.js
    robots_app/public/images
    robots_app/public
    robots_app/routes
    robots_app/routes/index.js
    robots_app/routes/users.js
    robots_app/public/stylesheets
    robots_app/public/stylesheets/style.css
    robots_app/views
    robots_app/views/index.ejs
    robots_app/views/error.ejs
    robots_app/bin
    robots_app/bin/www
    robots_app/public/javascripts

Don't worry if you're unfamiliar with the location and purpose of each of these files. Throughout this series, we will modify our application's directory structure and file names to resemble *Rails* conventions, and the similarities between *Express* and *Rails* will be become more clear.

### Installing Dependencies

The role of the `package.json` file is to declare package dependencies. *Rubyists* can think of it like a `Gemfile`.

```` sh
cd robots_app
npm install
````

This command installs all *NPM* module dependencies declared in `package.json`. It creates a new directory called `node_modules/`, which contains source code for all locally-installed modules. Before committing our project to version control, we'll want to ignore all files in this directory.

```` sh
touch .gitignore
````

Update `.gitignore` according to the following template:

```` sh
# .gitignore
node_modules/
````

## Running a Local Web Server

Run the development web server.

```` sh
DEBUG=robots_app:* npm start
````

You should now be able to visit the application's home page in your browser at `localhost:3000`.

After demonstrating the ability to view the application locally in a browser, stop the web server by typing `ctrl-c`.

## Upgrading Local Web Server

One shortcoming of the default web server is that it requires us to restart the server each time we make a change to one of our application's files. During development, this happens a lot, so we'll want to upgrade our development web server. We can use a module called *Nodemon*, which will automatically detect file changes and obviate our need to take manual action.

Let's install *Nodemon* globally.

```` sh
npm install nodemon -g
````

Modify the web server start script in `package.json` to invoke `nodemon` instead of `node`.

```` js
// package.json
...
  "scripts": {
    "start": "nodemon ./bin/www", // was: "start": "node ./bin/www",
  },
...
````

Restart the web server.

```` sh
DEBUG=robots_app:* npm start
````

Congratulations, now we're ready to [configure the application](/process-documentation/2016/04/09/node-for-rails-developers-part-3-express-configuration/).
