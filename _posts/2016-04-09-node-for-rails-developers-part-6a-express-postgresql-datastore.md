---
layout: post
title:  "Node.js for Rails developers, Part 6a (PostgreSQL Datastore)"
author: MJ Rossetti
published: false
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
 - postgresql
---

This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/). After enabling basic navigation, it's time to enable database functionality. This post describes the process of connecting the application to a *PostgreSQL* database.

## *PostgreSQL* Prerequisites

### Installing *PostgreSQL*

Follow [these instructions](http://data-creative.info/process-documentation/2015/07/18/how-to-set-up-a-mac-development-environment/#postgresql) to install *PostgreSQL* on your local machine, if necessary. Many *Rails* developers should already have *PostgreSQL* installed.

### Creating a New Database User

Create a new database user called `robot` and grant superuser privileges.

```` sh
psql
CREATE USER robot WITH ENCRYPTED PASSWORD 'r0b0t!';
ALTER USER robot CREATEDB;
ALTER USER robot WITH SUPERUSER;
\q
````

## Database Directory

*Rails* developers should be familiar with a `db/` directory which contains database-related scripts. We'll adopt these conventions for our *Node* app as well.

```` sh
mkdir -p db/
touch db/create.sql
````

Edit `db/create.sql` according to the following template:

```` sql
-- db/create.sql
````

## Creating a New Database



Finally, run the database creation script. *Rails* developers should find this familiar to running `rake db:create`.

```` sh
psql -U robot --password -d postgres -f $(pwd)/db/create.sql
````









## Installing Package Dependencies

To interface with *PostgreSQL*, we'll use the *pg* module.

*Knex* module provides a friendly way to write and run migrations, and execute queries.

 *Rails* developers can think of *Knex* as being similar to *ActiveRecord*,
  however you should note *Knex* does not include Object-Relational Mapping (ORM) capabilities like *ActiveRecord* does. This means there are no models with *Knex*.

```` sh
npm install pg --save
npm install knex --save
npm install knex -g
````

> We need to install *Knex* globally to invoke its command line utility.

## Configuring Database Connection

Initialize a new database configuration file.

```` sh
knex init .
mv knexfile.js db/config.js
touch db.js
````

Use the file templates below to configure the database connection and schema.

```` js
// db.js
````

```` js
// db/config.js (formerly known as knexfile.js
````




## Migrating the Database

Migrating the database creates all the tables we need. For this application, we only need one table, called `robots`.

```` sh
mkdir -p db/migrations/ # or name the directory db/migrate if you are yearning for exact rails conventions
````

Generate a new table migration.

```` sh
knex migrate:make create_robots
````

Edit `_________` according to the following template:
```` js
// ______
````

Run migrations.

```` sh
knex migrate:latest --knexfile db/config.js
````

## Seeding the Database

Seeding the database will populate it with example records.

```` sh
touch db/seed.js
````

Edit `db/seed.js` according to the following template:

```` js
// db/seed.js
````

Run the database-seeding script.

```` sh
knex seed:run --knexfile db/config.js
````






## Modifying Controller Actions
