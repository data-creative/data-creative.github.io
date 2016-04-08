---
layout: post
title:  "Node.js for Rails developers, Part 1 - An Introduction to Node and Express"
author: MJ Rossetti
published: true
img: nodejs-logo-green.png
repo_url: https://github.com/data-creative/express-robots
project_url: https://express-robots.herokuapp.com/
categories:
 - process-documentation
technologies:
 - git
 - node.js
 - npm
 - express.js
credits:
 - https://github.com/data-creative/express-robots/blob/master/CREDITS.md
 - https://github.com/data-creative/express-robots/blob/master/LEARNING.md
 - https://github.com/data-creative/express-robots/blob/master/README.md
 - http://data-creative.info/process-documentation/2015/07/18/how-to-set-up-a-mac-development-environment/
---

This post is the first in a three-part series for *Rails* developers who want to get started with [Node.js](https://nodejs.org/en/).

  + Part 1 - An Introduction to *Node* and *Express*
  + Part 2 - *PostgreSQL* and the *PEEN Stack*
  + Part 3 - *MongoDB* and the *MEEN Stack*

## Choose a Friendly Stack

The *MEAN Stack* is one of today's [popular technology stacks](http://techstacks.io/) for web development.

&nbsp; | Technology | Description
--- | --- | ---
**M** | *MongoDB* | key-value (noSQL) data store
**E** | *Express.js* | web server
**A** | *Angular.js* | client-side MVC framework
**N** | *Node.js*  | uses *JavaScript* as a server-side programming language

If you're a *Rails* developer, you might not have used any of these technologies before. In this case, you should endeavor to start small. Which are the minimum technologies you can use to get started? And which can you skip for now with the intention of exploring later as you build upon your foundation of understanding?

Well, your main objective is to gain familiarity with *Node*, so you can't skip that. *Node* let's you write in *JavaScript* on the server-side. For *Rails* developers, this takes the place of the *Ruby* language.

And you should know *Express* is an indispensable part of the stack, as it handles at minimum the web server and request-routing logic. *Rails* developers should think of *Express* as an application framework akin to *Rails*.

> This post focuses on setting up the basics for Express and Node.

Many *Rails* developers have a strong preference for relational databases. In this case, you can stick to *PostgreSQL* as a datastore, bypassing the immediate need to learn *MongoDB*.

> The second post focuses on connecting an Express app to a PostgreSQL datastore (PEEN Stack), while the third post focuses alternatively on connecting to a MongoDB datastore (MEEN Stack).

Unless you are already familiar with the *Angular* client-side MVC framework, you can skip that for now as well. Stick to basic views using the *EJS* template engine, which is similar to *ERB*.

> This series does not address Angular (MEAN Stack).

## Prerequisites

Let's get started.

### Install Node

Use *Homebrew* to install *Node*.

```` sh
brew install node
````

This will also install the primary *Node* package manager called *npm*. Rails developers should think of *npm* as being similar to *bundler*.

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

Later, we will modify our application's directory structure and configuration to more closely resemble *Rails* conventions.

## Install Package Dependencies

Next, install package dependences.
 The installation command's `--save` flag registers the module as a dependency in the `package.json` file. For *Rails* developers, just as *npm* equates to *bundler*, `package.json` is like a `Gemfile`.

```` sh
cd robots_app
npm install
````

You should now be able to run the web server and view the result in your browser at `localhost:3000`.

```` sh
DEBUG=robots_app:* npm start
````

Commit your project to git. Ignore all files in the `node_modules/` directory.

```` sh
touch .gitignore
atom .gitignore
````

```` sh
# .gitignore
node_modules/
````

```` sh
git init .
git commit -am "generating new express app"
````

## Upgrade Local Web Server

After demonstrating with success the ability to run the web application, shut down the web server with `ctrl-c`.

Revise `package.json` to use *nodemon* instead of *node* to start the web server.

````
// package.json
...
"scripts": {
    "start": "nodemon ./bin/www",
},
...
````

Restart the web server.

```` sh
DEBUG=robots_app:* npm start
````
