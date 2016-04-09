---
layout: post
title:  "Node.js for Rails developers, Part 1 (Choosing a Stack)"
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
credits:
 - https://scotch.io/tutorials/build-a-restful-api-using-node-and-express-4
---

> This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/).

## The MEAN Stack

The *MEAN Stack* is one of today's [popular technology stacks](http://techstacks.io/) for web development.

  + **M** stands for *MongoDB*, which is a key-value (no-*SQL*) datastore.
  + **E** stands for *Express.js*, a *Node.js* web server and application framework.
  + **A** stands for *Angular.js*, a client-side MVC framework.
  + **N** stands for *Node.js*, which runs *JavaScript* as a server-side programming language.

If you're a *Rails* developer, you may have never used any of these technologies before.
 Instead of trying to learn them all at once, modify your stack to minimize upfront learning efforts. After you learn the basics, you can then circle-back to build upon your foundation of knowledge and experience.

## Friendlier Stacks

So, which *MEAN Stack* technologies do you need to learn now, and which can you delay learning until later?

Well, since your main objective is to gain familiarity with *Node*, you can't skip that.
 And you should know *Express* is an indispensable part of the stack, as it handles at minimum the web server and request-routing logic.
 This leaves us with flexibility in the areas of the datastore and the front-end framework.

For the front-end framework, unless you're already familiar with the *Angular*, let's forget about it for now.
 We can start with basic views using the *EJS* template engine, which is similar to *ERB*. Most *Rails* developers should be familiar with *ERB* and will be happy to learn the syntax for *EJS* is almost nearly identical to *ERB*.

For the datastore, we have two basic options: *MongoDB* or a relational database. Most *Rails* developers should be familiar with open source relational databases like *MySQL* and *PostgreSQL*.

If you'd like to stick with a familiar *PostgreSQL* database, try the *PEEN Stack*:

  + **P** stands for *PostgreSQL*, a relational database.
  + **E** stands for *Express.js*, a *Node.js* web server and application framework.
  + **E** stands for *EJS*, a simple and familiar view template engine.
  + **N** stands for *Node.js*, which runs *JavaScript* as a server-side programming language.


Else if you're feeling adventurous, try the *MEEN Stack*:

  + **M** stands for *MongoDB*, which is a key-value (noSQL) data store.
  + **E** stands for *Express.js*, a *Node.js* web server and application framework.
  + **E** stands for *EJS*, a simple and familiar view template engine.
  + **N** stands for *Node.js*, which runs *JavaScript* as a server-side programming language.

You don't have to make a datastore decision until one of the later posts.
 First, let's [get started](/process-documentation/2016/04/09/node-for-rails-developers-part-2-node-and-express/) with *Node* and *Express*.
