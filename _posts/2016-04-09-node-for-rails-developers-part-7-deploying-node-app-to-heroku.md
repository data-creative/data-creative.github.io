---
layout: post
title:  "Node.js for Rails developers, Part 7 (Deploying to Heroku)"
author: MJ Rossetti
published: true
img: nodejs-logo-green.png
#repo_url: ______________
#project_url: https://express-sticky-notes.herokuapp.com/
categories:
 - process-documentation
technologies:
  - git
  - node.js
  - npm
  - express.js
  - postgresql
  - mongodb
  - heroku
---

This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/). In one of the two alternative previous posts, we connected our application to a datastore ([*PostgreSQL*](/process-documentation/2016/04/09/node-for-rails-developers-part-6a-express-postgresql-datastore/) or [*MongoDB*](/process-documentation/2016/04/09/node-for-rails-developers-part-6b-express-mongodb-datastore/)). In this post we will deploy our *Node* app to a production *Heroku* server. There are some minor differences in the process depending on your choice of datastore.

<hr>

## *Heroku* Prerequisites

Download [heroku toolbelt](https://toolbelt.heroku.com/) to enable `heroku` command line tools. Then login using your *Heroku* credentials.

```` sh
heroku login
````

## Creating a New *Heroku* Application

Create and name a new *Heroku* application.

```` sh
heroku create
heroku apps:rename new-app-name
````

## Configuring Production Environment Variables

Set environment variable(s), including a `SESSION_SECRET`.

```` sh
heroku config:set SESSION_SECRET=s0m3l0ngstr1ng123456
````

> *Heroku* sets other environment variables, including `NODE_ENV=production` during deploy.

## Installing Addons

Install a free datastore addon, depending on your datastore of choice:

*PostgreSQL*:

```` sh
heroku addons:create heroku-postgresql:hobby-dev
````

*MongoDB*:

```` sh
heroku addons:create mongolab:sandbox
````







## Application Configuration

After configuring the application's *Heroku* production environment, we need to reconfigure the application according to [*Heroku* specifications](https://devcenter.heroku.com/articles/nodejs-support).

### Production Web Server

Add a `Procfile` which specifies a command to start the production web server.

```` sh
touch Procfile
````

Edit `Procfile` according to the following template:

```` sh
# Procfile

web: node ./bin/www
````

> NOTE: We're not using *Nodemon* in production.

### Engines and Deploy Scripts

Modify `package.json` to include engines and deploy scripts, as necessary. Find your own engine versions with `node -v` and `npm -v`, respectively. Specify database preparation commands and other start-up scrips using the `scripts["heroku-prebuild"]` and `scripts["heroku-postbuild"]` configuration variables. These commands will be run during each deployment. Edit `package.json` according to the following template:

```` js
// package.json

{
  // ...
  "engines":{
    "node":"5.9.1",
    "npm":"3.7.3"
  },
  "scripts": {
    "start": "nodemon ./bin/www",
    "test": "echo This is for running tests like ... mocha.",
    "postinstall": "echo This is when you would run ... bower install && grunt build.",
    "heroku-prebuild": "echo This runs before Heroku installs your dependencies.",
    "heroku-postbuild": "echo This runs afterwards."
  },
  "dependencies": {
    // ...
  }
}
````

If you chose the *PostgreSQL* datastore, specify the following commands to migrate and seed the production database upon each deployment:

```` sh
"heroku-postbuild": "knex migrate:latest --knexfile db/config.js && knex seed:run --knexfile db/config.js"
````


### Production Session Storage

Our app's flash messaging requires session storage. The default session storage in development environments is `MemoryStore`, but *Heroku* [says](https://devcenter.heroku.com/articles/node-sessions#sessions-and-scaling) this is not a best practice in production applications. *Heroku* [recommends](https://devcenter.heroku.com/articles/node-sessions#storing-sessions-in-redis) *Redis* for session storage, but we don't need to add another dependency to our technology stack. Let's instead use the datastore we chose in the previous post.

#### Option A - *PostgreSQL* Session Store

If you chose a *PostgreSQL* datastore, let's use the `connect-session-knex` module to handle session storage.

```` sh
npm install connect-session-knex --save
````

Configure the application to use this module by editing the contents of `app.js` according to the following template:

```` js
// ...

var knex = require("./db"); // PG PRODUCTION ADDITION! enables pg database connection
var knexSessionStore = require('connect-session-knex')(session); // PG PRODUCTION ADDITION! uses pg database for session storage

// ...

var sessionStore = new knexSessionStore({
    knex: knex, // use existing knex configuration
    tablename: 'sessions' // create a database table with this name
}); // // PG PRODUCTION EDIT! was: var sessionStore = new session.MemoryStore;

// ...
````

As soon as the web server restarts, you should notice a new database table called `sessions`. Click around the application to see the table populate with new session information.


#### Option B - *MongoDB* Session Store

*COMING SOON*










## Deploying

Commit your changes and push them to a remote branch.

```` sh
git push origin master
````

Then deploy the app to production.

From master branch:

````sh
git push heroku master
````

From another branch:

```` sh
git push heroku mybranch:master
````


Congratulations. You should now be able to view your application on the web.

```` sh
heroku open
````










<hr>

That concludes this series of posts to help *Rails* developers learn *Node*. Thanks for following along. Leave a comment if you found the series helpful.
