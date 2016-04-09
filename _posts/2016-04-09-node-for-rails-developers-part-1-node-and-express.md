---
layout: post
title:  "Node.js for Rails developers, Part 1 (An Introduction to Node and Express)"
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
 - twitter-bootstrap
credits:
 - https://scotch.io/tutorials/build-a-restful-api-using-node-and-express-4
---

## Introduction

This post is the first in a five-part series for *Rails* developers who want to get started with [Node.js](https://nodejs.org/en/).

  + [Part 1 (An Introduction to *Node* and *Express*)](_________)
  + [Part 2 (Express Routing)](___________)
  + [Part 3 (Express Views and Controllers)](___________)
  + Part 4 - Datastore
    + [Part 4a (*PostgreSQL* Datastore)](____________)
    + [Part 4b (*MongoDB* Datastore)](____________)
  + [Part 5 (Deploying to Heroku)](___________)

## Choose a Friendly Stack

The *MEAN Stack* is one of today's [popular technology stacks](http://techstacks.io/) for web development.

&nbsp; | Technology | Description
--- | --- | ---
**M** | *MongoDB* | key-value (noSQL) data store
**E** | *Express.js* | web server
**A** | *Angular.js* | client-side MVC framework
**N** | *Node.js*  | uses *JavaScript* as a server-side programming language

If you're a *Rails* developer, you might not have used any of these technologies before. In this case, you should endeavor to start small. Which are the minimum technologies you can use to get started? And which can you skip for now with the intention of exploring later as you build upon your foundation of understanding?

Well, since your main objective is to gain familiarity with *Node*, you can't skip that. *Node* lets you write in *JavaScript* on the server-side. For *Rails* developers, this takes the place of the *Ruby* language.

And you should know *Express* is an indispensable part of the stack, as it handles at minimum the web server and request-routing logic. *Rails* developers should think of *Express* as an application framework akin to *Rails*.

Many *Rails* developers have a strong preference for relational databases. In this case, you can stick to *PostgreSQL* as a datastore, bypassing the immediate need to learn *MongoDB*.

> This series addresses both datastore options.

Unless you are already familiar with the *Angular* client-side MVC framework, you can skip that for now as well. Stick to basic views using the *EJS* template engine, which is similar to *ERB*.

> This series does not address Angular (MEAN Stack).

Let's get started.

## Install Node

Use *Homebrew* to install *Node*.

```` sh
brew install node
````

This will also install the primary *Node* package manager called *npm*. *Rails* developers should think of *npm* as being similar to *bundler*.

Use *npm* to install global modules. The difference between installing an *npm* module locally and installing it globally is that local installations are project-specific, whereas global installations allow the module to be invoked from the command line. Pass the `-g` flag when installing to denote a global installation.

```` sh
npm install express-generator -g
npm install nodemon -g
````

*Rails* developers should think of the *Express Generator* as serving many of the same functions as *Rails*' built-in generator methods.

When running a development web server, *Nodemon* obviates the need to restart the server each time a file is changed.

## Generate a New Express Application

You don't need to create from scratch all the files needed to make a new *Express* application. Instead, make use of the [Express Generator](http://expressjs.com/en/starter/generator.html) to create the initial directory skeleton for you. Think of this as the equivalent to running `rails g my_app`.

This will create a skeleton directory according to predefined *Express* conventions.

```` sh
--->> express robots_app --ejs

   create : robots_app
   create : robots_app/package.json
   create : robots_app/app.js
   create : robots_app/public/images
   create : robots_app/public
   create : robots_app/routes
   create : robots_app/routes/index.js
   create : robots_app/routes/users.js
   create : robots_app/public/stylesheets
   create : robots_app/public/stylesheets/style.css
   create : robots_app/views
   create : robots_app/views/index.ejs
   create : robots_app/views/error.ejs
   create : robots_app/bin
   create : robots_app/bin/www

   create : robots_app/public/javascripts
--->>
````

Throughout this series, we will modify our application's directory structure and configuration to more closely resemble *Rails* conventions.

## Install Default Package Dependencies

Next, install package dependences.

```` sh
cd robots_app
npm install
````

The `npm install` command installs all package dependencies stated in the `package.json` file. *Rails* developers can think of `package.json` as playing the same role as the `Gemfile`.

Notes on running `npm install`:
 + use the `--save` flag automatically registers the module as a dependency in the `package.json` file.
 + use the `-g` flag if you need to access a module's command line utility

## Ignore Node Modules in Source Control

Commit your project to git. Ignore all files in the `node_modules/` directory.

```` sh
touch .gitignore
atom .gitignore
````

Update `.gitignore` according to the following template:

```` sh
# .gitignore
````

Commit your changes.

```` sh
git init .
git commit -am "generating new express app"
````

## Run Local Web Server

You should now be able to run the web server and view the result in your browser at `localhost:3000`.

```` sh
DEBUG=robots_app:* npm start
````

## Upgrade Local Web Server

After demonstrating the ability to view the application locally in a browser, stop the web server by typing `ctrl-c`.

Revise `package.json`, specifically the section which specifies scripts. Modify the web server start script to use *nodemon* instead of *node*.

````
// package.json
````

Restart the web server.

```` sh
DEBUG=robots_app:* npm start
````
