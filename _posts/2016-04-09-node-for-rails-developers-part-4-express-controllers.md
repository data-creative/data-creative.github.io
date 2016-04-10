---
layout: post
title:  "Node.js for Rails developers, Part 4 (Express Controllers)"
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
credits:
 - https://scotch.io/tutorials/build-a-restful-api-using-node-and-express-4
---

> This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/).

## Create Controllers

Let's add the application's controllers.

```` sh
mv routes/index.js app/controllers/home_controller.js
touch app/controllers/robots_controller.js
rm -rf routes/
````

Edit the controllers using the below file templates:

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
    {
        id: 1,
        name:"c3po",
        description:"specializes in language translation",
        created_at: new Date("1976-06-06 13:23:23"),
        updated_at: new Date("1976-06-06 13:23:23") }
    ,
    {
        id: 2,
        name:"r2d2",
        description:"holds a secret message",              
        created_at: new Date("1977-07-07 23:10:10"),
        updated_at: new Date("1977-07-07 23:10:10")
    },
    {
        id: 3,
        name:"bb8",  
        description:"rolls around",                        
        created_at: new Date("2016-01-01 07:59:59"),
        updated_at: new Date("2016-01-01 07:59:59")
    },
]; // temporary variable assignment to pre-empt future database connection

/* INDEX */

router.get('/robots', function(req, res, next) {
    console.log("LIST", robots.length, "ROBOTS:", robots)
    res.render('robots/index', {
        page_title: 'Robots',
        robots: robots
    });
});

/* CREATE */

router.post('/robots', function(req, res, next) {
    console.log("CAPTURING FORM DATA:", req.body)
    console.log("CREATE ROBOT")
    req.flash('success', 'Created a New Robot');
    res.redirect('/robots')
});

/* NEW */
// this must come above the SHOW action else express will think the word 'new' is the :id

router.get('/robots/new', function(req, res, next) {
    console.log("NEW ROBOT")
    res.render('robots/new', {
        page_title: 'Add a new Robot'
    });
});

/* SHOW */

router.get('/robots/:id', function(req, res, next) {
    var robot_id = req.params.id;
    var robot = robots.find(function(r){ return r.id == robot_id; });
    if (typeof(robot) != "object") {
        console.log("COULDN'T SHOW ROBOT #"+robot_id)
        req.flash('danger', "DANGER - Couldn't find Robot #"+robot_id);
        req.flash('warning', "WARNING!");
        req.flash('info', "INFO!");
        req.flash('success', "SUCCESS!");
        res.redirect('/robots')
    } else {
      console.log("SHOW ROBOT:", robot)
      res.render('robots/show', {
        page_title: 'Robot #'+robot.id,
        robot: robot
      });
    };
});

/* EDIT */

router.get('/robots/:id/edit', function(req, res, next) {
  var robot_id = req.params.id
  var robot = robots.find(function(r){ return r.id == robot_id; });
  console.log("EDIT ROBOT:", robot)
  res.render('robots/edit', {
    page_title: 'Edit Robot #'+robot.id,
    robot: robot
  });
});

/* UPDATE */

router.post('/robots/:id/update', function(req, res, next) {
  console.log("CATURED FORM DATA", req.body)
  var robot_id = req.params.id
  var robot = robots.find(function(r){ return r.id == robot_id; });
  console.log("UPDATE ROBOT:", robot)
  req.flash('success', 'Updated Robot #'+robot_id );
  res.redirect('/robots')
});

/* DESTROY */

router.post('/robots/:id/destroy', function(req, res, next) {
  var robot_id = req.params.id
  var robot = robots.find(function(r){ return r.id == robot_id; });
  console.log("DELETED ROBOT:", robot)
  req.flash('success', 'Deleted Robot #'+robot_id );
  res.redirect('/robots')
});

module.exports = router;
````

With each `res.redirect() and res.render()`, we are referencing *EJS* views. Let's [create them now](/process-documentation/2016/04/09/node-for-rails-developers-part-5-express-views/).
