---
layout: post
title:  "Node.js for Rails developers, Part 1 (Choosing a Stack)"
author: MJ Rossetti
published: true
img: nodejs-logo-green.png
repo_url: https://github.com/data-creative/express-on-rails-starter-app/
project_url: https://express-on-rails-starter.herokuapp.com/
categories:
 - process-documentation
technologies:
 - node.js
 - express.js
 - postgresql
 - mongodb
---

> This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/).
 The goal of this post is to introduce popular *Node* technologies and provide *Rails*-friendly recommendations.

<hr>

## The *MEAN Stack*

The *MEAN Stack* is one of today's [popular technology stacks](http://techstacks.io/) for web development.

  + **M** stands for *MongoDB*, which is a key-value datastore.
  + **E** stands for *Express.js*, a *Node.js* web server and application framework.
  + **A** stands for *Angular.js*, a client-side MVC framework.
  + **N** stands for *Node.js*, which runs *JavaScript* as a server-side programming language.

If you're a *Rails* developer, you may have never used any of these technologies before.
 Instead of trying to learn them all at once, you can learn on a modified stack which plays to your strengths. This strategy will decrease your upfront learning efforts and help you build confidence as your understanding grows. After you learn the basics, you can then circle-back to learn the other technologies.

## Friendlier Stacks

So, which *MEAN Stack* technologies should you learn now, and which can you delay until later?

Let's assume your main objective is to gain familiarity with *Node*, so you wouldn't skip that. Plus, other parts of the stack, like *Express* are a part of the *Node* ecosystem. Using *Node* means *JavaScript* replaces *Ruby* as the main programming language in this stack.

*Express* is an indispensable part of the stack, as it handles the web server and application logic. *Rails* developers can think of *Express* as an application framework like *Rails*.

So far we're committed to learning *Node* and *Express*. But we have more flexibility in the areas of the datastore and the front-end framework.

For the datastore, there are two basic options: *MongoDB* or a relational database. Most *Rails* developers should be familiar with open source relational databases like *MySQL* and *PostgreSQL*. Because its easy enough to learn *MongoDB*, this series will cover both *MongoDB* and *PostgreSQL*.

For the front-end framework, unless you're already familiar with *Angular*, let's forget about it for now.
 Instead, we can use the *EJS* template engine to create basic views. *EJS* is almost nearly identical to *ERB*, the default template engine in *Rails*, so *Rails* developers should feel right at home.

### *PEEN Stack*

  + **P** stands for *PostgreSQL*, a relational database.
  + **E** stands for *Express.js*, a *Node.js* web server and application framework.
  + **E** stands for *EJS*, a simple and familiar view template engine.
  + **N** stands for *Node.js*, which runs *JavaScript* as a server-side programming language.

### *MEEN Stack*

  + **M** stands for *MongoDB*, which is a key-value data store.
  + **E** stands for *Express.js*, a *Node.js* web server and application framework.
  + **E** stands for *EJS*, a simple and familiar view template engine.
  + **N** stands for *Node.js*, which runs *JavaScript* as a server-side programming language.

Enough discussion. let's [get started](/process-documentation/2016/04/09/node-for-rails-developers-part-2-node-and-express/) with *Node* and *Express*.
