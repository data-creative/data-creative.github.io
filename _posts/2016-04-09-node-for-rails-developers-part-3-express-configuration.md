---
layout: post
title:  "Node.js for Rails developers, Part 3 (Express Configuration)"
author: MJ Rossetti
published: true
img: nodejs-logo-green.png
repo_url: https://github.com/data-creative/express-on-rails-starter-app/
project_url: https://express-on-rails-starter.herokuapp.com/
categories:
 - process-documentation
technologies:
 - git
 - node.js
 - npm
 - express.js
---

> This post is part of a series for *Rails* developers who want to get started with *Node.js*. After [generating a new application](/process-documentation/2016/04/09/node-for-rails-developers-part-2-node-and-express/), we're ready to configure it according to *Rails*-friendly conventions.

<hr>

## Directory Structure

### Application Directory

Like a *Rails* application, let's orient our application around an `app/` directory.

```` sh
mkdir app/
````

### Assets Directory

Rename the directory of static files to a new directory called `app/assets/`.

```` sh
mv public/ app/assets
````

### Controllers Directory

Make a new directory called `app/controllers/`. We will add and revise controller files later.

```` sh
mkdir -p app/controllers
mv routes/index.js app/controllers/home_controller.js
rm -rf routes/
````

### Views Directory

Make a new directory called `app/views/`. We will add and revise view files later.

```` sh
mkdir -p app/views
mv views/error.ejs app/views/_error.ejs
rm -rf views/
````















## Application Configuration

Now let's configure the application to recognize our new directory structure. We'll also configure additional modules to support a basic level of functionality.

Revise the application configuration file, `app.js`, to resemble the template below. Observe additions to the original file denoted by `ADDITION!`, changes denoted by `EDIT!`, and in-line comments for explanation.

```` js
// app.js

var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');
var session = require('express-session'); // ADDITION! enables session storage; required for flash messages
var flash = require('connect-flash'); // ADDITION! enables flash messages
var moment = require('moment-timezone'); // ADDITION! enables date string formatting

var home_routes = require('./app/controllers/home_controller'); // EDIT! recognizes the home controller file, app/controllers/home_controller. was: var routes = require('./routes/index');
var robot_routes = require('./app/controllers/robots_controller'); // EDIT! recognizes the robots controller file, app/controllers/robots_controller. was: var users = require('./routes/users');

var sessionStore = new session.MemoryStore; // ADDITION! the default memory store for sessions in the development environment

var app = express();

app.locals.moment = moment;  // ADDITION! this makes moment available as a variable in every EJS page
app.locals.title = "Robots App!" // ADDITION! set a common title for all EJS views

// view engine setup
app.set('views', path.join(__dirname, 'app/views')); // EDIT! recognizes view templates stored in the app/views directory. was: app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

// uncomment after placing your favicon in /public
//app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'app/assets'))); // EDIT! recognizes static files stored in the app/assets directory. was: app.use(express.static(path.join(__dirname, 'public')));

app.use(session({
    cookie: { maxAge: 60000 },
    store: sessionStore,
    secret: process.env.SESSION_SECRET || 'robots-session-secret',
    name: 'robots-session-name',
    resave: true,
    saveUninitialized: true
 })); // ADDITION! configures session storage; required for flash messages

app.use(flash()); // ADDITION! enables the application to use the flash module

app.use(function (req, res, next) {
    res.locals.messages = require('express-messages')(req, res);
    next();
}); // ADDITION! enables storage of flash messages and makes them accessable to views. must be placed below app.use(cookieParser()) section, and above app.use('/', routes) section

app.use('/', home_routes); // EDIT! orients the location of home paths relative to the root url, "/". was: app.use('/', routes);
app.use('/', robot_routes); // EDIT! orients the location of robot paths relative to the root url, "/". some people might want to orient these reletive to "/robots" instead, in which case you'd have to remove "robots/" from your robots controller paths. was: app.use('/users', users);

// catch 404 and forward to error handler
app.use(function(req, res, next) {
    var err = new Error('Not Found');
    err.status = 404;
    next(err);
});

// error handlers

// development error handler
// will print stacktrace
if (app.get('env') === 'development') {
    app.use(function(err, req, res, next) {
        res.status(err.status || 500);
        res.render('_error', { // EDIT! recognizes a renamed view template for errors. file name was: res.render('error', {
            message: err.message,
            error: err
        });
    });
}

// production error handler
// no stacktraces leaked to user
app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('_error', { // EDIT! recognizes a renamed view template for errors. file name was: res.render('error', {
        message: err.message,
        error: {}
    });
});

module.exports = app;
````

## Package Installations

You'll notice we configured the application to require additional modules. Let's install them now.

```` sh
npm install express-session --save
npm install connect-flash --save
npm install express-messages --save
npm install moment-timezone --save
````

> NOTE: Passing the `--save` option automatically registers the module as a dependency in the application's `package.json` file.

OK, now we're ready to use these modules in our [controllers and views](/process-documentation/2016/04/09/node-for-rails-developers-part-4-express-controllers/).
