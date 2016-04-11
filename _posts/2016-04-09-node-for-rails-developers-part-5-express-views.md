---
layout: post
title:  "Node.js for Rails developers, Part 5 (Express Views)"
author: MJ Rossetti
published: true
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
 - twitter-bootstrap
---

This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/). After [creating the application's controllers](/process-documentation/2016/04/09/node-for-rails-developers-part-4-express-controllers/), its time to create the views.

## Creating Views

### Shared Views

Let's add view partials to represent re-usable parts of the page.

```` sh
touch app/views/_head.ejs
touch app/views/_header.ejs
touch app/views/_bootstrap_flash_messages.ejs
touch app/views/_footer.ejs
````

Edit the files according to the following templates:

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
  <!-- FLASH MESSAGES -->
  <%- messages('_bootstrap_flash_messages') %>

  <!-- SITE TITLE AND NAVIGATION -->
  <h1><a href="/"><%= title %></a></h1>
  <a type="button" class="btn btn-primary pull-right" href="/robots/new">
    <span class="glyphicon glyphicon-plus"></span> new
  </a>
</header>

<h2><%= page_title %></h2>
````

```` html
<!-- app/views/_bootstrap_flash_messages.ejs -->

<% if(messages){ %>
  <% Object.keys(messages).forEach(function (type) { %>
    <% messages[type].forEach(function (message) { %>
      <div class="alert alert-<%= type %> alert-dismissible" role="alert">
        <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <%= message %>
      </div>
    <% }) %>
  <% }) %>
<% } %>
````

```` html
<!-- app/views/_footer.ejs -->

<hr style="margin-top:2em;">
<footer>
  <p>
    <a href="#">source</a>
  </p>
</footer>

<script src="https://code.jquery.com/jquery-1.12.0.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js" integrity="sha384-0mSbJDEHialfmuBBQP6A4Qrprq5OVfW37PRR3j5ELqxss1yVqOtnepnHVP9aJ7xS" crossorigin="anonymous"></script>
<script type="text/javascript">

    console.log("bootstrap flash")
    $().alert('close') // closes data-dismiss="alert" http://getbootstrap.com/javascript/#alerts-methods

</script>
````

> NOTE: Underscored file names denote view partials (incomplete pages).

### Robot Views

Now let's add more views to handle basic CRUD functionality for our robots app, including robot-specific views and partials.

```` sh
mkdir -p app/views/robots/
touch app/views/robots/index.ejs
touch app/views/robots/show.ejs
touch app/views/robots/new.ejs
touch app/views/robots/edit.ejs
touch app/views/robots/_form.ejs

mkdir -p app/views/robots/table
touch app/views/robots/table/_row.ejs
touch app/views/robots/table/_header_row.ejs
````

Edit the files according to the following templates:

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
  <% var action = '/robots/'+robot.id+'/update' %>
  <% var method = 'POST' %>
  <% var robot_name = robot.name %>
  <% var robot_description = robot.description %>
<% } else { %>
  <% var action = '/robots/' %>
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
<% destroy_robot_path = '/robots/'+robot.id+'/destroy' %>

<tr>
  <td><%= robot.id %></td>
  <td><a href="<%= robot_path %>"><%= robot.name %></a></td>
  <td><%= robot.description %></td>
  <td><%= moment(robot.created_at).tz(moment.tz.guess(robot.created_at)).format('YYYY-MM-DD [at] HH:mm:ss zz') %></td>
  <td><%= moment(robot.updated_at).tz(moment.tz.guess(robot.updated_at)).format('YYYY-MM-DD [at] HH:mm:ss zz') %></td>
  <td>
    <form action="<%= edit_robot_path %>" method='GET'>
      <button class='btn btn-warning' type='submit'>
        <span class="glyphicon glyphicon-pencil"></span> edit
      </button>
    </form>
  </td>
  <td>
    <form action="<%= destroy_robot_path %>" method='POST'>
      <button class='btn btn-danger' type='submit'>
        <span class="glyphicon glyphicon-trash"></span> delete
      </button>
    </form>
  </td>
</tr>
````

> NOTE: You'll notice undesired repetition of code across `index.ejs`, `show.ejs`, `new.ejs`, and `edit.ejs`. The `express-ejs-layouts` module enables us to use a common application layout, but it conflicts with our custom bootstrap-styled flash messages, so in preference for app functionality over code DRY-ness, we will stick with these views for now.















## Restarting Local Server

At this point, you should be able to click around the application without breaking anything, even though database functionality is still missing.

![robots app index page screenshot](/img/posts/express-robots-index-bootstrap-flash.png)

It's time to enhance this application's functionality by connecting a datastore. Choose your own adventure (6a or 6b):

 + [Part 6a (*PostgreSQL* Datastore)](/process-documentation/2016/04/09/node-for-rails-developers-part-6a-express-postgresql-datastore/) -- COMING SOON
 + [Part 6b (*MongoDB* Datastore)](/process-documentation/2016/04/09/node-for-rails-developers-part-6b-express-mongodb-datastore/) -- COMING SOON
