---
layout: post
title:  "Node.js for Rails developers, Part 4b (MongoDB Datastore)"
author: MJ Rossetti
published: true
img: nodejs-logo-green.png
repo_url: ______________
project_url: https://express-sticky-notes.herokuapp.com/
categories:
 - process-documentation
technologies:
 - git
 - node.js
 - npm
 - express.js
 - twitter-bootstrap
 - mongodb
---

## Recap

This post is the fourth in a five-part series for *Rails* developers who want to get started with [Node.js](https://nodejs.org/en/).

  + [Part 1 (An Introduction to *Node* and *Express*)](_________)
  + [Part 2 (Express Routing)](___________)
  + [Part 3 (Express Views and Controllers)](___________)
  + Part 4 - Datastore
    + [Part 4a (*PostgreSQL* Datastore)](____________)
    + [Part 4b (*MongoDB* Datastore)](____________)
  + [Part 5 (Deploying to Heroku)](___________)

In this post, we will configure a *MongoDB* datastore. Let's call it a *MEEN Stack*, maybe.

&nbsp; | Technology | Description
--- | --- | ---
**M** | *MongoDB* | key-value (noSQL) data store
**E** | *Express.js* | web server
**E** | *EJS* | plain familiar view templates
**N** | *Node.js*  | uses *JavaScript* as a server-side programming language
