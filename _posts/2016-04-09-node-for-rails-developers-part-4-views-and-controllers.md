---
layout: post
title:  "Node.js for Rails developers, Part 3 (Express Views and Controllers)"
author: MJ Rossetti
published: false
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
 - twitter-bootstrap
credits:
 - https://scotch.io/tutorials/build-a-restful-api-using-node-and-express-4
---

> This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/).



### Create Controllers

Now let's add the application's controllers using the file templates below.

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



















## Restart the server


```` sh
npm install moment-timezone --save
npm install connect-flash --save
npm install express-messages --save
npm install express-session --save
````

At this point, you should be able to click around the application without breaking anything, but some of the functionality is still missing.

![robots app index page screenshot](/img/posts/express-robots-index.png)

OK, it's time to enhance this application's functionality by connecting to a datastore.

Choose your own adventure, (2a or 2b) depending on the desired datastore:

 + [Part 2a (*PostgreSQL* Datastore)](/process-documentation/2016/04/07/node-for-rails-developers-part-2-postgresql-peen-stack/)
 + [Part 2b (*MongoDB* Datastore)](/process-documentation/2016/04/08/node-for-rails-developers-part-3-mongodb-meen-stack/)
