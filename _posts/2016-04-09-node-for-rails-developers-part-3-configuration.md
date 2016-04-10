---
layout: post
title:  "Node.js for Rails developers, Part 3 (Configuration)"
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

It's time to revise the application's directory structure to confirm more closely with *Rails* conventions.

```` sh
mv public/ assets
mkdir -p app/controllers
mkdir -p app/views
````

## Configure Application

Let's configure the application to recognize our desired directory structure.
 Revise `app.js`, to resemble the template below. Observe additions to the original file denoted by `ADDITION!` and changes denoted by `EDIT!`

```` js
// app.js
````
