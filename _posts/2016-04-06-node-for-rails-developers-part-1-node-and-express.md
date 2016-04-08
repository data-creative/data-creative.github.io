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

Well, your main objective is to gain familiarity with *Node*, so you can't skip that. *Node* let's you write in *JavaScript* on the server-side. For *Rails* developers, this takes the place of the *Ruby* language.

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
 The installation command's `--save` flag registers the module as a dependency in the `package.json` file. For *Rails* developers, just as *npm* equates to *bundler*, `package.json` is like a `Gemfile`.

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

It's a same time to flesh-out the application's views according to *Rails* conventions.

```` sh
mkdir -p app/views
````



### Create Controllers

Now let's add the application's controllers.
























































### Create View Partials

Let's add view partials using the templates provided below:

> NOTE: the underscored names are a personal preference for view partials. Feel free to use non-underscorized names.

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

### Create CRUD Views

Now let's add more views to handle basic CRUD functionality for our robots app, including robot-specific view partials:

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
  <th>Title</th>
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
  <td><%= moment(robot.created_at).tz(moment.tz.guess(robot.created_at)).format('YYYY-MM-DD @ HH:mm:ss zz') %></td>
  <td><%= moment(robot.updated_at).tz(moment.tz.guess(robot.updated_at)).format('YYYY-MM-DD @ HH:mm:ss zz') %></td>
  <td>
    <form action=<%= edit_robot_path %> method='GET'>
      <button class='btn btn-warning' type='submit'>
        <span class="glyphicon glyphicon-pencil"></span> edit
      </button>
    </form>
  </td>
  <td>
    <form action=<%= destroy_robot_path %> method='POST'>
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
  <% var action = '/robot/' + robot.id + '/update' %>
  <% var robot_name = robot.name %>
  <% var robot_description = robot.description %>
<% } else { %>
  <% var action = '/robots/new' %>
<% }; %>

<form class="form-horizontal" method="POST" action=<%= action %>>
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

### Configure Routing

Finally, we will configure the application to recognize the location of files according to our revised directory structure.




















































Congratulations. Now let's make this application more dynamic by connecting to a datastore and implementing flash messages.

Choose your own adventure, depending on the desired datastore:

 + [Continue to Part 2 (*PostgreSQL* and the *PEEN Stack*) -->](/process-documentation/2016/04/07/node-for-rails-developers-part-2-peen-stack/)
 + OR [Skip to Part 3 (*MongoDB* and the *MEEN Stack*) -->](/process-documentation/2016/04/08/node-for-rails-developers-part-3-mongodb-meen-stack/)
