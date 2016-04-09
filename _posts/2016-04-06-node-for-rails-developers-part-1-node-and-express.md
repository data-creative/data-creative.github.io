---
layout: post
title:  "Node.js for Rails developers, Part 1 (An Introduction to Node and Express)"
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
 - https://scotch.io/tutorials/build-a-restful-api-using-node-and-express-4
---

This post is the first in a three-part series for *Rails* developers who want to get started with [Node.js](https://nodejs.org/en/).

  + [Part 1 (An Introduction to *Node* and *Express*)](/process-documentation/2016/04/06/node-for-rails-developers-part-1-node-and-express/)
  + [Part 2 (*PostgreSQL* and the *PEEN Stack*)](/process-documentation/2016/04/07/node-for-rails-developers-part-2-postgresql-peen-stack/)
  + [Part 3 (*MongoDB* and the *MEEN Stack*)](/process-documentation/2016/04/08/node-for-rails-developers-part-3-mongodb-meen-stack/)

## Choose a Friendly Stack

The *MEAN Stack* is one of today's [popular technology stacks](http://techstacks.io/) for web development.

&nbsp; | Technology | Description
--- | --- | ---
**M** | *MongoDB* | key-value (noSQL) data store
**E** | *Express.js* | web server
**A** | *Angular.js* | client-side MVC framework
**N** | *Node.js*  | uses *JavaScript* as a server-side programming language

If you're a *Rails* developer, you might not have used any of these technologies before. In this case, you should endeavor to start small. Which are the minimum technologies you can use to get started? And which can you skip for now with the intention of exploring later as you build upon your foundation of understanding?

Well, since your main objective is to gain familiarity with *Node*, you can't skip that. *Node* lets you write in *JavaScript* on the server-side. For *Rails* developers, this takes the place of the *Ruby* language.

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

This will also install the primary *Node* package manager called *npm*. *Rails* developers should think of *npm* as being similar to *bundler*.

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
 The installation command's `--save` flag registers the module as a dependency in the `package.json` file. For *Rails* developers, just as *npm* is like *bundler*, `package.json` is like the `Gemfile`.

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

After demonstrating the ability to view the application locally in a browser, stop the web server by typing `ctrl-c`.

Revise `package.json`, specifically the section which specifies scripts. Modify the web server start script to use *nodemon* instead of *node*.

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





































## Create Controllers and Views

It's time to revise the application's directory structure to confirm more closely with *Rails* conventions.

```` sh
mkdir -p app/controllers
mkdir -p app/views
````

Let's also install the `moment-timezone` module, which we will use in our views to format date strings.

```` sh
npm install moment-timezone --save
````

### Configure Routing

Let's configure the application to recognize the location of files according to our desired directory structure. Revise `app.js`, to resemble the template below. Observe additions denoted by `ADDITION!` and changes denoted by `EDIT!`

```` js
// app.js

var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');
var moment = require('moment-timezone'); // ADDITION!

var home_routes = require('./app/controllers/home_controller'); // EDIT! was: var routes = require('./routes/index');
var robot_routes = require('./app/controllers/robots_controller'); // EDIT! was: var users = require('./routes/users');

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
app.use(express.static(path.join(__dirname, 'public')));

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

### Create Controllers

Now let's add the application's controllers using the templates below.

```` sh
mv routes/index.js app/controllers/home_controller.js
touch app/controllers/robots_controller.js
rm -rf routes/
````


```` js
// app/controllers/home_controller.js

var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  console.log("VISITED THE HOME PAGE")
  res.redirect('/robots')
});

module.exports = router;
````

```` js
// app/controllers/robots_controller.js

var express = require('express');
var router = express.Router();

var robots = [
  {id: 1, name:"c3po", description:"specializes in language translation", created_at: new Date("1976-06-06 13:23:23"), updated_at: new Date("1976-06-06 13:23:23") },
  {id: 2, name:"r2d2", description:"holds a secret message",              created_at: new Date("1977-07-07 23:10:10"), updated_at: new Date("1977-07-07 23:10:10") },
  {id: 3, name:"bb8",  description:"rolls around",                        created_at: new Date("2016-01-01 07:59:59"), updated_at: new Date("2016-01-01 07:59:59") },
]; // temporary placeholder for future database connection

router.route('/robots')

    /* INDEX */

    .get(function(req, res, next) {
      console.log("FOUND", robots.length, "ROBOTS")
      res.render('robots/index', {
        page_title: 'Robots',
        robots: robots
      });
    })

    /* CREATE */

    .post(function(req, res, next) {
        console.log("CAPTURING FORM DATA:", req.body);
        console.log("CREATED A NEW ROBOT");
        res.redirect('/robots');
    });

/* NEW (this must come above the SHOW action or else express will think 'new' is the robot :id). */

router.get('/robots/new', function(req, res, next) {
  res.render('robots/new', {
    page_title: 'Add a new Robot'
  });
});

/* EDIT */

router.get('/robots/:id/edit', function(req, res, next) {
    var robot_id = req.params.id;
    var robot = robots.find(function(r){ return r.id == robot_id; });

    if ( typeof(robot) == "undefined" ) {
        console.log("ERROR - COULDN'T FIND ROBOT #"+robot_id)
        res.redirect('/robots')
    } else {
        console.log("EDITING A ROBOT", robot)
        res.render('robots/edit', {
          page_title: 'Edit Robot #'+robot.id,
          robot: robot
        });
    };
});

router.route('/robots/:id')

    /* SHOW */

    .get(function(req, res, next) {
        var robot_id = req.params.id;
        var robot = robots.find(function(r){ return r.id == robot_id; });
        if ( typeof(robot) == "undefined" ) {
            console.log("ERROR - COULDN'T FIND ROBOT #"+robot_id)
            res.redirect('/robots')
        } else {
            res.render('robots/show', {
              page_title: 'Robot #'+robot.id,
              robot: robot
            });
        };
    })

    /* UPDATE */

    .put(function(req, res, next) {
        console.log("CATURED FORM DATA", req.body)
        var robot_id = req.params.id
        console.log("UPDATED ROBOT #"+robot_id)
        res.redirect('/robots')
    })

    /* DESTROY */

    .delete(function(req, res, next) {
        var robot_id = req.params.id
        console.log("DELETING ROBOT #"+robot_id)
        res.redirect('/robots')
    });

module.exports = router;
````

### Create View Partials

Let's add view partials using the templates provided below.

```` sh
mv views/error.ejs app/views/_error.ejs
rm -rf views/
touch app/views/_footer.ejs
touch app/views/_head.ejs
touch app/views/_header.ejs
````

```` html
<!-- app/views/_head.ejs -->

<head>
  <title><%= title %></title>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" integrity="sha384-1q8mTJOASx8j1Au+a5WDVnPi2lkFfwwEAa8hDDdjZlpLegxhjVME1fgjWPGmkzs7" crossorigin="anonymous">
  <link rel='stylesheet' href='/stylesheets/style.css' />
</head>
````

```` html
<!-- app/views/_header.ejs -->

<header>
  <!-- FYI: this is where flash messages will go -->
  <h1><a href="/"><%= title %></a></h1>
  <a type="button" class="btn btn-primary pull-right" href="/robots/new">
    <span class="glyphicon glyphicon-plus"></span> new
  </a>
</header>

<h2><%= page_title %></h2>
````

```` html
<!-- app/views/_footer.ejs -->

<hr style="margin-top:2em;">
<footer>
  <p>
    <a href="#" title="link to source code">source</a>
  </p>
</footer>
````

> NOTE: if you don't like underscored file names, feel free to use non-underscorized file names instead.

### Create Robot Views

Now let's add more views to handle basic CRUD functionality for our robots app, including robot-specific views and view partials.

```` sh
mkdir -p app/views/robots/
mkdir -p app/views/robots/table
touch app/views/robots/index.ejs
touch app/views/robots/show.ejs
touch app/views/robots/table/_row.ejs
touch app/views/robots/table/_header_row.ejs
touch app/views/robots/new.ejs
touch app/views/robots/edit.ejs
touch app/views/robots/_form.ejs
````

In this example, the index page and the show page share a table partial.

```` html
<!-- app/views/robots/index.ejs -->

<!DOCTYPE html>
<html>
  <% include ../_head %>
  <body>
    <% include ../_header %>

    <table class="table table-bordered table-hover table-responsive" style="width:100%">
      <% include table/_header_row %>
      <% robots.forEach(function(robot){ %>
        <% include table/_row %>
      <% }); %>
    </table>

    <% include ../_footer %>
  </body>
</html>
````

```` html
<!-- app/views/robots/show.ejs -->

<!DOCTYPE html>
<html>
  <% include ../_head %>
  <body>
    <% include ../_header %>

    <table class="table table-bordered table-hover table-responsive" style="width:100%">
      <% include table/_header_row %>
      <% include table/_row %>
    </table>

    <% include ../_footer %>
  </body>
</html>
````


```` html
<!-- app/views/robots/table/_header_row.ejs -->

<tr>
  <th>Id</th>
  <th>Name</th>
  <th>Description</th>
  <th>Created At</th>
  <th>Updated At</th>
  <th>&nbsp;</th>
  <th>&nbsp;</th>
</tr>
````

```` html
<!-- app/views/robots/table/_row.ejs -->

<% robot_path = '/robots/'+robot.id %>
<% edit_robot_path = '/robots/'+robot.id+'/edit' %>

<tr>
  <td><%= robot.id %></td>
  <td><a href="<%= robot_path %>"><%= robot.name %></a></td>
  <td><%= robot.description %></td>
  <td><%= moment(robot.created_at).tz(moment.tz.guess(robot.created_at)).format('YYYY-MM-DD [at] HH:mm:ss zz') %></td>
  <td><%= moment(robot.updated_at).tz(moment.tz.guess(robot.updated_at)).format('YYYY-MM-DD [at] HH:mm:ss zz') %></td>
  <td>
    <form action=<%= edit_robot_path %> method='GET'>
      <button class='btn btn-warning' type='submit'>
        <span class="glyphicon glyphicon-pencil"></span> edit
      </button>
    </form>
  </td>
  <td>
    <form action=<%= robot_path %> method='DELETE'>
      <button class='btn btn-danger' type='submit'>
        <span class="glyphicon glyphicon-trash"></span> delete
      </button>
    </form>
  </td>
</tr>
````

The new page and edit page share a form partial.

```` html
<!-- app/views/robots/new.ejs -->

<!DOCTYPE html>
<html>
  <% include ../_head %>
  <body>
    <% include ../_header %>

    <% include _form %>

    <% include ../_footer %>
  </body>
</html>
````

```` html
<!-- app/views/robots/edit.ejs -->

<!DOCTYPE html>
<html>
  <% include ../_head %>
  <body>
    <% include ../_header %>

    <% include _form %>

    <% include ../_footer %>
  </body>
</html>
````

```` html
<!-- app/views/robots/_form.ejs -->

<% if(locals.robot){ %>
  <% var action = '/robots/' %>
  <% var method = 'PUT' %>
  <% var robot_name = robot.name %>
  <% var robot_description = robot.description %>
<% } else { %>
  <% var action = '/robots/new' %>
  <% var method = 'POST' %>
<% }; %>

<form class="form-horizontal" method="<%= method %>" action="<%= action %>">
  <div class="form-group">
    <label for="robotName" class="col-sm-2 control-label">Name</label>
    <div class="col-sm-10">
      <input type="text" class="form-control" name="robotName" placeholder="My Robot" value="<%= robot_name %>">
    </div>
  </div>

  <div class="form-group">
    <label for="robotDescription" class="col-sm-2 control-label">Description</label>
    <div class="col-sm-10">
      <textarea class="form-control" rows="3" name="robotDescription" placeholder="All the things..."><%= robot_description %></textarea>
    </div>
  </div>

  <div class="form-group">
    <div class="col-sm-offset-2 col-sm-10">
      <button type="submit" class="btn btn-success">Submit</button>
    </div>
  </div>
</form>
````

At this point, you should be able to click around the application without breaking anything, but some of the functionality is still missing.

![robots app index page screenshot](/img/posts/express-robots-index.png)

OK, it's time to enhance this application's functionality by connecting to a datastore and implementing flash messages.

Choose your own adventure, depending on the desired datastore:

 + [Continue to Part 2 (*PostgreSQL* and the *PEEN Stack*)](/process-documentation/2016/04/07/node-for-rails-developers-part-2-peen-stack/)
 + OR [Skip to Part 3 (*MongoDB* and the *MEEN Stack*)](/process-documentation/2016/04/08/node-for-rails-developers-part-3-mongodb-meen-stack/)
