---
layout: post
title:  "Node.js for Rails developers, Part 3 - MongoDB and the MEEN Stack"
author: MJ Rossetti
published: true
img: nodejs-logo-green.png
repo_url: https://github.com/data-creative/express-sticky-notes/
project_url: https://express-sticky-notes.herokuapp.com/
categories:
 - process-documentation
technologies:
 - git
 - node.js
 - npm
 - express.js
 - mongodb
credits:
 - https://github.com/data-creative/express-robots/blob/master/CREDITS.md
 - https://github.com/data-creative/express-robots/blob/master/LEARNING.md
 - https://github.com/data-creative/express-robots/blob/master/README.md
 - http://data-creative.info/process-documentation/2015/07/18/how-to-set-up-a-mac-development-environment/
---

This post is the second in a three-part series for *Rails* developers who want to get started with [Node.js](https://nodejs.org/en/).

  + Part 1 - An Introduction to *Node* and *Express*
  + Part 2 - *PostgreSQL* and the *PEEN Stack*
  + Part 3 - *MongoDB* and the *MEEN Stack*

In Part 1, we set up a default web application and demonstrated the ability to run a local web server.

In Part 2, we configured *PostgreSQL* as a datastore. In this post, we will ignore the second post, and instead build upon the first post using an alternate datastore, *MongoDB*. Let's call this a *MEEN Stack*.

&nbsp; | Technology | Description
--- | --- | ---
**P** | *MongoDB* | key-value (noSQL) data store
**E** | *Express.js* | web server
**E** | *EJS* | plain familiar view templates
**N** | *Node.js*  | uses *JavaScript* as a server-side programming language

![sticky notes app screenshot](/img/posts/express-sticky-notes.png)
