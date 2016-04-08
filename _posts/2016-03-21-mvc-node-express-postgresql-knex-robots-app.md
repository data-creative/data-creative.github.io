---
layout: post
title:  "Node.js for Rails developers"
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

Many *Rails* developers have a strong preference for relational databases. In this case, you can stick to *PostgreSQL* as a datastore, bypassing the immediate need to learn *MongoDB*.

And unless you are already familiar with the *Angular* client-side MVC framework, you can skip that for now as well. Stick to basic views using the *EJS* template engine, which is similar to *ERB*.

For lack of a better term, we're going to use the *PEEN Stack*:

&nbsp; | Technology | Description
--- | --- | ---
**P** | *PostgreSQL* | reliable open source relational database
**E** | *Express.js* | web server
**E** | *EJS* | plain familiar view templates
**N** | *Node.js*  | uses *JavaScript* as a server-side programming language

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

*Knex* is mainly used for writing migrations which form the basis of the database schema. We will also use it for its database query interface. *Rails* developers should think of *Knex* as being similar to *ActiveRecord*, however you should note *Knex* does not include Object-Relational Mapping (ORM) capabilities like *ActiveRecord*.

When running a development web server, *Nodemon* obviates the need to restart the server each time a file is changed.

### Install and Configure Database

Follow [these instructions](http://data-creative.info/process-documentation/2015/07/18/how-to-set-up-a-mac-development-environment/#postgresql) to install *PostgreSQL* on your local machine, if necessary. Many *Rails* developers should already have *PostgreSQL* installed.

Create a new database user called `robot` and grant superuser privileges.

```` sh
psql
CREATE USER robot WITH ENCRYPTED PASSWORD 'r0b0t!';
ALTER USER robot CREATEDB;
ALTER USER robot WITH SUPERUSER;
\q
````

## Generate a New Application

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

After demonstrating with success the ability to run the web application, shut down the web server with `ctrl-c`, and reconfigure the `___/____.js` script to use *nodemon* instead.

````
// ___/___.js
________
________
________
````

Restart the web server.

```` sh
nodemon _______
````

## Install Remaining Package Dependencies

As mentioned in the introduction, for this project, we will also declare package dependencies on the *PostgreSQL* library.

````
npm install pg --save
````










<hr>














## Create Database

Create a new directory in your application directory called `db/`. *Rails* developers should be familiar with having a `db/` directory in their applications which contains database-related scripts.

Place inside your irectory a new database creation script called `db/create.sql`:

```` sh
mkdir db/
touch db/create.sql
atom db/create.sql
````

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

## Migrate and Seed the Database

Migrate the database. This creates all the tables we need. In the case of this application, we only need one table, called `robots`.

```` sh
knex migrate:latest --knexfile db/config.js
````

Seed the database.

```` sh
knex seed:run --knexfile db/config.js
````
