---
layout: post
title:  "Node.js for Rails developers, Part 5 (Express Views)"
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
 - twitter-bootstrap
---

> This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/).

## Creating Views

### Shared Views

Let's add view partials, which represent re-usable parts of the page.

```` sh
touch app/views/_footer.ejs
touch app/views/_head.ejs
touch app/views/_header.ejs
````

```` html
<!-- app/views/_head.ejs -->


````

```` html
<!-- app/views/_header.ejs -->

````

```` html
<!-- app/views/_footer.ejs -->

````

> NOTE: if you don't like underscored file names, feel free to use non-underscorized file names instead.

### Robot Views

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

At this point, you should be able to click around the application without breaking anything, but some of the functionality is still missing.

![robots app index page screenshot](/img/posts/express-robots-index.png)

OK, it's time to enhance this application's functionality by connecting to a datastore.

Choose your own adventure, (8a or 8b) depending on the desired datastore:

 + [Part 8a (*PostgreSQL* Datastore)](/process-documentation/2016/04/09/node-for-rails-developers-part-6a-express-postgresql-datastore)
 + [Part 8b (*MongoDB* Datastore)](/process-documentation/2016/04/09/node-for-rails-developers-part-6b-express-mongodb-datastore)
