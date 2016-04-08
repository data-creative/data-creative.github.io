---
layout: post
title:  "How to make a Node.js app - An introduction for Rails developers"
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
 - postgresql
credits:
 - https://github.com/data-creative/express-robots/blob/master/CREDITS.md
 - https://github.com/data-creative/express-robots/blob/master/LEARNING.md
 - https://github.com/data-creative/express-robots/blob/master/README.md
 - http://data-creative.info/process-documentation/2015/07/18/how-to-set-up-a-mac-development-environment/
---

This post is for *Rails* developers who want to get started with [Node.js](https://nodejs.org/en/).

## Choose a Friendly Stack

The *MEAN Stack* is one of today's [popular technology stacks](http://techstacks.io/) for web development. It consists of:

  + *Mongodb*  -- key-value (noSQL) data store
  + *Express.js* -- web server
  + *Angular.js* -- client-side MVC framework
  + *Node.js*  -- uses *JavaScript* as a server-side programming language

If you're a *Rails* developer, you might not have used any of these technologies before. In this case, you should endeavor to start small. Which are the minimum technologies you can use to get started? And which can you skip for now with the intention of exploring later as you build upon your foundation of understanding?

Well, your main objective is to gain familiarity with *Node*, so you can't skip that. *Node* let's you write in *JavaScript* on the server-side. For rails developers, this takes the place of the *Ruby* language.

And you should know *Express* is an indispensable part of the stack, as it handles at minimum the web server and request-routing logic. *Rails* developers should think of *Express* as an application framework akin to *Rails*.

Many *Rails* developers have a strong preference for relational databases. In this case, you can stick to *PostgreSQL* as a datastore, bypassing the immediate need to learn *Mongodb*.

And unless you are already familiar with the *Angular* client-side MVC framework, you can skip that for now as well.
 Stick to basic views. Like many *Rails* developers, you may be familiar with writing views in the *ERB* template language. In this case, you'll be glad to hear you can configure *Express* to use *EJS* as a view template engine. *EJS* looks and feels the same as *ERB*.

## Prerequisites

### Install Node

Use *Homebrew* to install *Node*.

```` sh
brew install node
````

This will also install the primary *Node* package manager called *npm*. Rails developers should think of *npm* as being similar to *bundler*.

Use *npm* to install global modules. The difference between installing an *npm* module locally and installing it globally is that local installations are project-specific, whereas global installations allow the module to be invoked from the command line. Pass the `-g` flag when installing to denote a global installation.

```` sh
npm install express-generator -g
npm install -g knex
npm install nodemon -g
````

*Rails* developers should think of the *Express Generator* as serving many of the same functions as *Rails*' built-in generator methods.

*Knex* is mainly used for writing migrations which form the basis of the database schema. We will also use it for its database query interface. *Rails* developers should think of *Knex* as being similar to *ActiveRecord*.

When running a development web server, *Nodemon* obviates the need to restart the server each time a file is changed.

### Install and Configure Database

Follow [these instructions](http://data-creative.info/process-documentation/2015/07/18/how-to-set-up-a-mac-development-environment/#postgresql) to install *PostgreSQL* on your local machine, if necessary. Many *Rails* developers should already have *PostgreSQL* installed.

Create a new database user and grant superuser privileges.

```` sh
psql
CREATE USER robot WITH ENCRYPTED PASSWORD 'r0b0t!';
ALTER USER robot CREATEDB;
ALTER USER robot WITH SUPERUSER;
\q
````

## Generate a New Application

You don't need to create from scratch all the files needed to make a new *Express* application. Instead, make use of the [Express Generator](http://expressjs.com/en/starter/generator.html) to create the initial directory skeleton for you. Think of this as the equivalent to running `rails g my_app`.

```` sh
express my_app # similar to `rails g my_app`.
````

This will create a skeleton directory according to predefined *Express* conventions.

```` sh
cd my_app
ls -al
````


## Create Database

Create a new directory in your application directory called `db/`. *Rails* developers should be familiar with having a `db/` directory in their applications which contains database-related scripts.

Place inside your database directory a new database creation script called `db/create.sql`

```` sql
-- db/create.sql
DROP DATABASE IF EXISTS robots_dev;
CREATE DATABASE robots_dev;
GRANT ALL PRIVILEGES ON DATABASE robots_dev to robot;
````

Use the new database user to run the database creation script. *Rails* developers should find this familiar to running `rake db:create`.

```` sh
psql -U robot --password -d postgres -f $(pwd)/db/create.sql
````
