---
layout: post
title:  "How to make a Node.js app - An introduction for Rails developers"
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
 - postgresql
credits:
 - https://github.com/data-creative/express-robots/blob/master/CREDITS.md
 - https://github.com/data-creative/express-robots/blob/master/LEARNING.md
 - https://github.com/data-creative/express-robots/blob/master/README.md
---

This post is for *Rails* developers who want to get started with [Node.js](https://nodejs.org/en/).

## Choose a Friendly Stack

The *MEAN Stack* is one of today's [popular technology stacks](http://techstacks.io/) for web development. It consists of:

  + *Mongodb*  -- key-value (noSQL) data store
  + *Express.js* -- web server
  + *Angular.js* -- client-side MVC framework
  + *Node.js*  -- uses *JavaScript* as a server-side programming language

If you're a *Rails* developer like me, you might not have used any of these technologies before. In this case, you should endeavor to start small. Which are the minimum technologies you can use to get started? And which can you skip for now with the intention of exploring later as you build upon your foundation of understanding?

Well, your main objective is to gain familiarity with *Node*, so you can't skip that.

And you should know *Express* is an indispensable part of the stack, as it handles at minimum the web server and request-routing logic.

If you're like me, you have a strong preference for relational databases. In this case, you can stick to *PostgreSQL* as a datastore, bypassing the immediate need to learn *Mongodb*.

And unless you are already familiar with the *Angular* client-side MVC framework, you can skip that for now as well.
 Stick to basic views. Like many *Rails* developers, you may be familiar with writing views in the *ERB* template language. In this case, you'll be glad to hear you can configure *Express* to use *EJS* as a view template engine. *EJS* looks and feels the same as *ERB*, and I'm told they have common ancestry.

## Prerequisites

Install

## Generate

```` sh
express my_app
````
