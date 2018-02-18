---
layout: post
title:  "Node.js for Rails developers, Part 6a (PostgreSQL Datastore)"
author: MJ Rossetti
published: true
img: nodejs-logo-green.png
repo_url: https://github.com/data-creative/express-on-rails-starter-app/
project_url: https://express-on-rails-starter.herokuapp.com/
categories:
 - reference-docs
technologies:
 - git
 - node.js
 - npm
 - express.js
 - twitter-bootstrap
 - postgresql
 - homebrew
series: node-js-for-rails-developers
subtitle: "Part 6a: PostgreSQL Datastore"
---

> This post is part of a [series](/series/{{ page.series }}) for *Rails* developers who want to get started with *Node.js*.
  After [enabling basic navigation](/reference-docs/2016/04/09/node-for-rails-developers-part-5-express-views/), it's time to enable database functionality. This post describes the process of connecting the application to a *PostgreSQL* database.

<hr>

## *PostgreSQL* Prerequisites

### Installing *PostgreSQL*

Follow [these instructions](http://data-creative.info/reference-docs/2015/07/18/how-to-set-up-a-mac-development-environment/#postgresql) to install *PostgreSQL* on your local machine using *Homebrew*, if necessary. Many *Rails* developers should already have *PostgreSQL* installed.

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

*Rails* developers should be familiar with a `db/` directory which contains database-related scripts, including migrations. We'll adopt these same conventions in our application.

```` sh
mkdir -p db/
mkdir -p db/migrations/
mkdir -p db/seeds/
touch db/create.sql
````

Edit `db/create.sql` according to the following template:

```` sql
-- db/create.sql

DROP DATABASE IF EXISTS robots_dev;
CREATE DATABASE robots_dev;
GRANT ALL PRIVILEGES ON DATABASE robots_dev to robot;
````

## Creating a New Database

Run the database creation script. *Rails* developers can think of this as replacing the `rake db:create` command.

```` sh
psql -U robot --password -d postgres -f $(pwd)/db/create.sql
````

At this point, you should be able to login to *PostgreSQL* to confirm existence of a database called `robots_dev`.









## Installing Package Dependencies

To interface with *PostgreSQL*, we'll use the *PG* module. We'll also use the [*Knex*](https://github.com/tgriesser/knex) module to run migrations and execute queries. *Rails* developers can think of *Knex* as being similar to *ActiveRecord* but should note *Knex* does not include Object-Relational Mapping (ORM) capabilities like *ActiveRecord* does. This means there are no models with *Knex*.

```` sh
npm install pg --save
npm install knex --save
npm install knex -g
````

## Configuring Database Connection

Initialize a new database configuration file.

```` sh
knex init .
mv knexfile.js db/config.js
touch db.js
````

Use the file templates below to configure the database connection.

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
            directory: __dirname+"/seeds"
        }
    },

    production: {
        client: 'pg',
        connection: process.env.DATABASE_URL,
        migrations: {
            directory: __dirname+"/migrations"
        },
        seeds: {
            directory: __dirname+"/seeds"
        }
    }
};
````


## Generating Migrations

It's time to specify the database schema. For this application, we only need one table, called `robots`. Use *Knex* to generate a new table migration called `create_robots`.

```` sh
knex migrate:make create_robots --knexfile db/config.js
````

This command creates a file named something like `db/migrations/20160410205446_create_robots.js`.
Edit this migration file according to the following template:

```` js
// db/migrations/20160410205446_create_robots.js (NOTE: your timestamp will differ)

exports.up = function(knex, Promise) {
    return Promise.all([
        knex.schema.createTable("robots", function (table) {
            table.increments(); // auto-incrementing integer id
            table.string("name").notNullable().unique();
            table.text("description").notNullable();

            table.timestamp("created_at").defaultTo(knex.raw('now()')).notNullable();
            table.timestamp("updated_at").defaultTo(knex.raw('now()')).notNullable();
            // table.timestamps(); // creates created_at and updated_at attributes, but with no logic ...
        })
    ]);
};

exports.down = function(knex, Promise) {
    return Promise.all([
        knex.schema.dropTable("robots")
    ]);
};
````

## Migrating the Database

Run migrations. *Rails* developers can think of this as replacing the `rake db:migrate` command.

```` sh
knex migrate:latest --knexfile db/config.js
````

At this point, you should be able to login to *PostgreSQL* to confirm existence of a table in the `robots_dev` database called `robots`.

## Seeding the Database

Create a new script which will populate the `robots` table with example records.

```` sh
touch db/seeds/insert_robots.js
````

Edit `db/seeds/insert_robots.js` according to the following template:

```` js
// db/seeds/insert_robots.js

exports.seed = function(knex, Promise) {
    var robotPromises = [];
    var robots = [
        {name:"c3po", description:"specializes in language translation"},
        {name:"r2d2", description:"holds a secret message"},
        {name:"bb8",  description:"rolls around"}
    ];

    // destroy all records in the table...
    var destruction_promise = knex('robots').del()
    robotPromises.push(destruction_promise);

    // add a new record for each robot...
    var insertion_promise = knex('robots').insert(
        robots, 'id' // auto-increment the id instead of setting it
    );
    robotPromises.push(insertion_promise);

    return Promise.all(robotPromises);
};
````

> NOTE: A [Promise](https://www.promisejs.org/) represents the result of an asynchronous operation.


Run the database-seeding script.

```` sh
knex seed:run --knexfile db/config.js
````

At this point, you should be able to login to *PostgreSQL* to confirm existence of three records in the `robots` table.




## Modifying Controller Actions


We want the application to display robots from the database, not from a hard-coded variable. We also want to be able to use our views to add, edit, and delete robots from the database. Let's connect our robots controller actions to the database.

Modify `app/controllers/robots_controller.js` according to the following template:

```` js
var express = require('express');
var router = express.Router();
var knex = require("../../db");

var create_robot_path = '/robots/';
function updateRobotPath(robot_id){
    return '/robots/'+robot_id+'/update';
};

/* INDEX */

router.get('/robots', function(req, res, next) {
    knex("robots")
        .orderBy('id', 'desc')
        .then(function(bots){
            console.log("LIST", bots.length, "ROBOTS:", bots)
            res.render('robots/index', {
                page_title: 'Robots',
                robots: bots
            });
        });
});

/* CREATE */

router.post('/robots', function(req, res, next) {
    console.log("CAPTURING FORM DATA:", req.body)
    var robot_name = req.body.robotName;
    var robot_description = req.body.robotDescription;
    if (!robot_name || !robot_description) {
        console.log("DETECTED BLANK (BUT NOT NULL) ATTRIBUTE VALUES")
        if (!robot_name) {
            req.flash('danger', "Robot name can't be blank. Please revise and re-submit.")
        }

        if (!robot_description) {
            req.flash('danger', "Robot description can't be blank. Please revise and re-submit.")
        }

        res.render('robots/new', {
            page_title: 'Add a new Robot',
            form_action: create_robot_path,
            robot: {name: robot_name, description: robot_description} // pass-back attempted values to the form in case one was not blank
        });
    } else {
        knex('robots')
            .where({name: robot_name}) // look-up robot by unique name
            .then(function(bots){
                if (bots.length > 0) {
                    var bot = bots[0];
                    console.log(bot)
                    req.flash('danger', 'Found an Existing Robot named '+robot_name );
                    res.render('robots/new', {
                        page_title: 'Add a new Robot',
                        form_action: create_robot_path,
                        robot: {name: robot_name, description: robot_description} // pass-back attempted values to the form in case one was not blank
                    });
                } else {
                    knex('robots')
                        .insert([{'name': robot_name, 'description': robot_description}], 'id')
                        .then(function(bot_id){
                            console.log(bot_id)
                            req.flash('info', 'Created a New Robot named '+robot_name );
                            res.redirect('/robots')
                    });
                }
            });
    }
});

/* NEW */
// this must come above the SHOW action else express will think the word 'new' is the :id

router.get('/robots/new', function(req, res, next) {
    console.log("NEW ROBOT")
    res.render('robots/new', {
        page_title: 'Add a new Robot',
        form_action: create_robot_path
    });
});

/* SHOW */

router.get('/robots/:id', function(req, res, next) {
    var robot_id = req.params.id;
    knex("robots")
        .where({id: robot_id})
        .then(function(bots){
            if (bots.length > 0) {
                var bot = bots[0];
                console.log("SHOW ROBOT:", bot);
                res.render('robots/show', {
                    page_title: 'Robot #'+bot.id,
                    robot: bot
                });
            } else {
                console.log("COULDN'T SHOW ROBOT #"+robot_id);
                req.flash('danger', "Couldn't find Robot #"+robot_id);
                res.redirect('/robots');
            }
        });
});

/* EDIT */

router.get('/robots/:id/edit', function(req, res, next) {
    var robot_id = req.params.id;
    knex("robots")
        .where({id: robot_id})
        .then(function(bots){
            if (bots.length > 0) {
                var bot = bots[0];
                console.log("EDIT ROBOT:", bot);
                res.render('robots/edit', {
                    page_title: 'Edit Robot #'+bot.id,
                    robot: bot,
                    form_action: updateRobotPath(bot.id)
                });
            } else {
                console.log("COULDN'T FIND ROBOT #"+robot_id);
                req.flash('danger', "Couldn't find Robot #"+robot_id);
                res.redirect('/robots');
            }
        });
});

/* UPDATE */

router.post('/robots/:id/update', function(req, res, next) {
    console.log("CATURED FORM DATA", req.body)
    var robot_id = req.params.id;
    var robot_name = req.body.robotName;
    var robot_description = req.body.robotDescription;

    if (!robot_name || !robot_description) {
        console.log("DETECTED BLANK (BUT NOT NULL) ATTRIBUTE VALUES")
        if (!robot_name) {
            req.flash('danger', "Robot name can't be blank. Please revise and re-submit.")
        }

        if (!robot_description) {
            req.flash('danger', "Robot description can't be blank. Please revise and re-submit.")
        }

        res.render('robots/edit', {
            page_title: 'Edit Robot #'+robot_id,
            form_action: updateRobotPath(robot_id),
            robot: {name: robot_name, description: robot_description} // pass-back attempted values to the form in case one was not blank
        });
    } else {
        knex('robots')
            .where({id: robot_id})
            .update({name: robot_name, description: robot_description})
            .then(function(number_of_affected_rows){
                console.log("UPDATED", number_of_affected_rows, "ROBOT")
                req.flash('success', 'Updated Robot #'+robot_id );
                res.redirect('/robots')
            });
    }

});

/* DESTROY */

router.post('/robots/:id/destroy', function(req, res, next) {
    var robot_id = req.params.id
    knex("robots")
        .where({id: robot_id})
        .del()
        .then(function(number_of_affected_rows){
            console.log("DELETED", number_of_affected_rows, "ROBOT")
            req.flash('success', 'Deleted Robot #'+robot_id );
            res.redirect('/robots')
        });
});

module.exports = router;
````

> NOTE: In *Knex* and other promise-based modules, the [`.then()` method](http://knexjs.org/#Promises-then) specifies a block of code to be executed after the query has been executed asynchronously.




















<hr>

## Checkpoint

At this point you should be able to use the front-end interface to create, read, update, and display robots.


![robots app index page screenshot with new robot](/assets/img/posts/express-robots-index-with-created-robot.png)


Nice job. After a few more steps, we'll be ready to [push this application to production](/reference-docs/2016/04/09/node-for-rails-developers-part-7-deploying-node-app-to-heroku/).
