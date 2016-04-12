---
layout: post
title:  "Node.js for Rails developers, Part 7 (Deploying to Heroku)"
author: MJ Rossetti
published: false
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
  - heroku
---

This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/). In one of the two alternative previous posts, we connected our application to a datastore (*PostgreSQL* or *MongoDB*). In this post we will deploy our *Node* app to a production *Heroku* server. There are some minor differences in preparation, depending on the datastore.


## Heroku Prerequisites

Download [heroku toolbelt](https://toolbelt.heroku.com/) to enable `heroku` command line tools.

```` sh
heroku login
````

## Configuring Heroku

Create and name a new *Heroku* application.

```` sh
heroku create
heroku apps:rename new-app-name
````

Set environment variable(s). Setting `NODE_ENV` is technically unnecessary because *Heroku* does this during deploy.

```` sh
heroku config:set NODE_ENV=production SESSION_SECRET=s0m3l0ngstr1ng123456
````

## Installing Addons

### Option A - *PostgreSQL* Addon

Install a free *PostgreSQL* production database instance.

```` sh
heroku addons:create heroku-postgresql:hobby-dev
````

### Option B - *MongoDB* Addon

Install a free *MongoLab* production datastore instance.

```` sh
heroku addons:create mongolab:sandbox
````







## Deploy Scripts

Modify `package.json` to include versions and deploy scripts, as necessary. Find your versions with `node -v` and `npm -v`, respectively.

Let's tell *Heroku* to run our database migration and population scripts upon each deploy. *Heroku* scans the `package.json` file to find commands specified in scripts called `heroku-prebuild` and `heroku-postbuild`.

Edit `package.json` according to the following template:

```` js
// package.json

{
  "name": "robots_app",
  "version": "0.0.0",
  "private": true,
  "engines":{
    "node":"5.4.0", // specify your own version of node here
    "npm":"3.3.12" // specify your own version of npm here
  },
  "scripts": {
    "start": "nodemon ./bin/www",
    "heroku-prebuild": "echo This runs before Heroku installs your dependencies.",
    "heroku-postbuild": "knex migrate:latest --knexfile db/config.js && knex seed:run --knexfile db/config.js"
  },
  "dependencies": {
  ...
````

## Production Web Server

Add a `Procfile` to specify the command which will start the production web server.

```` sh
touch Procfile
````

Edit `Procfile` according to the following template:

```` sh
# Procfile

web: node ./bin/www
````

## Production Session Storage

Our app's flash messaging requires session storage. The default session storage in development environments is `MemoryStore`, but *Heroku* [says](https://devcenter.heroku.com/articles/node-sessions#sessions-and-scaling) this is not a best practice in production applications.

*Heroku* [recommends](https://devcenter.heroku.com/articles/node-sessions#storing-sessions-in-redis) *Redis* for session storage, but we don't need to add another dependency to our technology stack. Let's instead use the datastore we chose in the previous post.

### Option A - *PostgreSQL* Session Store

If you chose a *PostgreSQL* datastore, let's configure a new table called `sessions` in our database to hold session information.


### Option B - *MongoDB* Session Store

If you chose a *MongoDB* datastore, let's configure a new collection called `sessions` to hold session information.








Set environment variable(s). Setting `NODE_ENV` is technically unnecessary because heroku does it automatically during deploy.

```` sh
heroku config:set NODE_ENV=production SESSION_SECRET=s0m3l0ngstr1ng123456
````

Ensure postgresql addon is installed.

```` sh
heroku addons:create heroku-postgresql:hobby-dev
````




## Deploying

Deploy the app to production.

```` sh
git push heroku master
````

Congratulations. You should now be able to view your application on the web.

```` sh
heroku open
````

Thanks for following this series. Leave a comment if you found it helpful.
