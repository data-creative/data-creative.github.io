---
layout: post
title:  "Node.js for Rails developers, Part 2 (Node and Express)"
author: MJ Rossetti
published: true
img: nodejs-logo-green.png
repo_url: ______________
project_url: https://express-robots.herokuapp.com/
categories:
 - process-documentation
technologies:
 - git
 - node.js
 - npm
 - express.js
---

> This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/).

## Install Node

We're ready to get started with *Node* and *Express*. Unless you've already installed *Node* on your computer, use *Homebrew* to install it now.

```` sh
which node # to detect installation
brew install node
````

The *Homebrew* installation command will also install *NPM*, the primary package manager for *Node*. *Rails* developers can think of *NPM* as playing a similar role as *Bundler*.


## Install Express

We're going to use *NPM* to install the [*Express Generator*](http://expressjs.com/en/starter/generator.html) module, which includes *Express* and provides a helpful application-generation command.

Install *Express Generator* globally.

```` sh
npm install express-generator -g
````

> Note: You'll notice throughout this series sometimes we'll use *NPM* to install modules locally while other times we will install them globally. The difference between installing an *NPM* module locally and installing it globally is that local installations are project-specific, whereas global installations allow the module to be invoked from the command line. Passing the `-g` flag denotes a global installation. Passing the `--save` option automatically registers the module as a dependency in the application's `package.json` file.

## Generate a New Express Application

Use the *Express Generator* to generate a skeleton directory structure for a new *Express* app.

```` sh
express robots_app --ejs
````

This command should create the following files:

  + `robots_app`
  + `robots_app/package.json`
  + `robots_app/app.js`
  + `robots_app/public/images`
  + `robots_app/public`
  + `robots_app/routes`
  + `robots_app/routes/index.js`
  + `robots_app/routes/users.js`
  + `robots_app/public/stylesheets`
  + `robots_app/public/stylesheets/style.css`
  + `robots_app/views`
  + `robots_app/views/index.ejs`
  + `robots_app/views/error.ejs`
  + `robots_app/bin`
  + `robots_app/bin/www`
  + `robots_app/public/javascripts`

Don't worry if you're unfamiliar with the location and purpose of each of these files. Throughout this series, we will modify our application's directory structure to more closely resemble *Rails* conventions, and things will become more clear.

## Install Dependencies

Before we can visit our new app in a browser, we need to install package dependences. The `npm install` command installs all package dependencies stated in the `package.json` file. *Rails* developers can think of `package.json` as playing a similar role as the `Gemfile`.

```` sh
cd robots_app
npm install
````

After running this command, you should see a new directory called `node_modules/` which contains source code for all local package dependencies.

## Run Local Web Server

Run the development web server.

```` sh
DEBUG=robots_app:* npm start
````

You should now be able to visit the application's home page in your browser at `localhost:3000`.

After demonstrating the ability to view the application locally in a browser, stop the web server by typing `ctrl-c`.

## Upgrade Local Web Server

One shortcoming of the default web server is that it requires us to restart the server each time we make a change to one of our application's files. During development, this happens a lot, so we'll want to upgrade our development web server. We can use a module called *Nodemon*, which will automatically detect file changes and obviate our need to take manual action.

Let's install *Nodemon* globally.

```` sh
npm install nodemon -g
````

Modify the web server start script in `package.json` to invoke `nodemon` instead of `node`.

````
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

## Commit

Before committing our project to version control, we want to ignore all files in the `node_modules/` directory.

```` sh
touch .gitignore
atom .gitignore
````

Update `.gitignore` according to the following template:

```` sh
# .gitignore
node_modules/
````

Finally, commit your changes.

```` sh
git init .
git commit -am "generating a new express app"
````

Congratulations, now you're ready to [configure your application](/process-documentation/2016/04/09/node-for-rails-developers-part-3-configuration/) to more closely resemble *Rails* conventions.
