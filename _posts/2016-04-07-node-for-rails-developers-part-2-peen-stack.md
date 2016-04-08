---
layout: post
title:  "Node.js for Rails developers, Part 2 - PostgreSQL and the PEEN Stack"
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

This post is the second in a three-part series for *Rails* developers who want to get started with [Node.js](https://nodejs.org/en/).

  + Part 1 - An Introduction to *Node* and *Express*
  + Part 2 - *PostgreSQL* and the *PEEN Stack*
  + Part 3 - *MongoDB* and the *MEEN Stack*

In Part 1, we set up a default web application and demonstrated the ability to run a local web server. In this post, we will build upon the first post by connecting the application to a *PostgreSQL* datastore. Let's call this a *PEEN Stack*.

&nbsp; | Technology | Description
--- | --- | ---
**P** | *PostgreSQL* | reliable open source relational database
**E** | *Express.js* | web server
**E** | *EJS* | plain familiar view templates
**N** | *Node.js*  | uses *JavaScript* as a server-side programming language

## Install PostgreSQL and Create a Database User

Follow [these instructions](http://data-creative.info/process-documentation/2015/07/18/how-to-set-up-a-mac-development-environment/#postgresql) to install *PostgreSQL* on your local machine, if necessary. Many *Rails* developers should already have *PostgreSQL* installed.

Create a new database user called `robot` and grant superuser privileges.

```` sh
psql
CREATE USER robot WITH ENCRYPTED PASSWORD 'r0b0t!';
ALTER USER robot CREATEDB;
ALTER USER robot WITH SUPERUSER;
\q
````


## Install Node Packages

We're going to use the *Knex* module primarily to write and execute migrations which form the basis of the database schema. We will also use it for its database query interface. *Rails* developers should think of *Knex* as being similar to *ActiveRecord*, however you should note *Knex* does not include Object-Relational Mapping (ORM) capabilities like *ActiveRecord*.

```` sh
npm install knex -g
npm install knex --save
````

We will also declare package dependencies on the *PostgreSQL* library.

````
npm install pg --save
````

## Configure Models and Migrations

Initialize *Knex*.

```` sh
knex init .
````

Create a database directory structure which resembles *Rails* conventions.

```` sh
mkdir -p db/
mv knexfile.js db/config.js
touch db.js
touch db/create.sql
touch db/seed.js # touch db/seeds/create_robots.js
mkdir -p db/migrations/ # or name the directory db/migrate if you are yearning for exact rails conventions
````

Use the file templates below to configure the database connection and schema.

```` js
// db.js

var config      = require('./db/config');
var env         = process.env.NODE_ENV || 'development';
var knex        = require('knex')(config[env]);

module.exports = knex;
````

```` js
// db/config.js (formerly known as knexfile.js

module.exports = {
  development: {
    client: 'pg',
    connection: {
      user: 'robot',
      password: 'r0b0t!',
      database: 'robots_dev'
    },
    migrations: {
      directory: __dirname+"/migrations"
    },
    seeds: {
      directory: __dirname // __dirname+"/seeds"
    }
  },

  production: {
    client: 'pg',
    connection: process.env.DATABASE_URL,
    migrations: {
      directory: __dirname+"/migrations"
    },
    seeds: {
      directory: __dirname // __dirname+"/seeds"
    }
  }
};
````

```` js
// db/create.sql
DROP DATABASE IF EXISTS robots_dev;
CREATE DATABASE robots_dev;
GRANT ALL PRIVILEGES ON DATABASE robots_dev to robot;
````

```` js
// db/seed.js

exports.seed = function(knex, Promise) {
  var robotPromises = [];
  var robots = [{name:"r2d2"}, {name:"c3po"}, {name:"bb8"}]

  robots.forEach(function(bot){
    var robot_name = bot["name"]
    console.log('Robot name: ' + robot_name);

    knex('robots').where({name: robot_name}).then(function(robots){
      if (robots.length > 0) {
        console.log('Found existing robot named '+robot_name);
      } else {
        var insertion_promise = knex('robots').insert([{'name': robot_name}], 'id')
        robotPromises.push(insertion_promise);
      };
    });
  });

  return Promise.all(robotPromises);
};
````




































## Create the Database

Create a new directory in your application directory called `db/`. *Rails* developers should be familiar with having a `db/` directory in their applications which contains database-related scripts.

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
