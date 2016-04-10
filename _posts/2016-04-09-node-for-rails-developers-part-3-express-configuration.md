---
layout: post
title:  "Node.js for Rails developers, Part 3 (Express Configuration)"
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
---

> This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/).


## Modify Directory Structure

It's time to revise the application's directory structure to conform more closely with *Rails* conventions.

```` sh
mv public/ assets
mkdir -p app/controllers
mkdir -p app/views
````

## Configure Application

Now let's configure the application to recognize our desired directory structure.
 Revise `app.js`, to resemble the template below. Observe additions to the original file denoted by `ADDITION!` and changes denoted by `EDIT!`

```` js
// app.js

var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');
var session = require('express-session'); // ADDITION!
var flash = require('connect-flash'); // ADDITION!
var moment = require('moment-timezone'); // ADDITION!

var home_routes = require('./app/controllers/home_controller'); // EDIT! was: var routes = require('./routes/index');
var robot_routes = require('./app/controllers/robots_controller'); // EDIT! was: var users = require('./routes/users');

var sessionStore = new session.MemoryStore; // ADDITION! the default memory store for development applications

var app = express();

app.locals.moment = moment;  // ADDITION! this makes moment available as a variable in every EJS page
app.locals.title = "Robots App!" // ADDITION! set a common title for all EJS views

// view engine setup
app.set('views', path.join(__dirname, 'app/views')); // EDIT! was: app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

// uncomment after placing your favicon in /public
//app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'assets'))); // EDIT! was: app.use(express.static(path.join(__dirname, 'public')));

app.use(session({
   cookie: { maxAge: 60000 },
   store: sessionStore,
   secret: process.env.SESSION_SECRET || 'robots-session-secret',
   name: 'robots-session-name',
   resave: true,
   saveUninitialized: true
 })); // ADDITION!

app.use(flash()); // ADDITION!

app.use(function (req, res, next) {
 res.locals.messages = require('express-messages')(req, res);
 next();
}); // ADDITION! include flash messages (must be placed below session and cookie parser, and above ... app.use('/', routes);)

app.use('/', home_routes); // EDIT! was: app.use('/', routes);
app.use('/', robot_routes); // EDIT! was: app.use('/users', users);

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
    res.render('_error', { // EDIT! was: res.render('error', {
      message: err.message,
      error: err
    });
  });
}

// production error handler
// no stacktraces leaked to user
app.use(function(err, req, res, next) {
  res.status(err.status || 500);
  res.render('_error', { // EDIT! was: res.render('error', {
    message: err.message,
    error: {}
  });
});


module.exports = app;
````

## Install Package Dependencies

You'll notice we configured the application to require additional modules. Let's install them all now.

```` sh
npm install express-session --save
npm install connect-flash --save
npm install express-messages --save
npm install moment-timezone --save
````

OK, now we're ready to use these libraries in our [controllers and views](/process-documentation/2016/04/09/node-for-rails-developers-part-4-express-views-and-controllers/).
