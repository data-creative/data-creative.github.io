---
layout: post
title:  "Node.js for Rails developers, Part 7 (Deploying to Heroku)"
author: MJ Rossetti
published: false
img: nodejs-logo-green.png
#repo_url: ______________
#project_url: https://express-sticky-notes.herokuapp.com/
categories:
 - process-documentation
technologies:
  - git
  - node.js
  - npm
  - express.js
  - heroku
---

This post is part of a series for *Rails* developers who want to get started with [*Node.js*](https://nodejs.org/en/). In one of the two alternative previous posts, we connected our application to a datastore (*PostgreSQL* or *MongoDB*). In this post we will deploy our *Node* app to a production *Heroku* server. There are some minor differences in preparation, depending on the datastore.

## Production Session Storage

Our app's flash messaging requires session storage. The default session storage in development environments is `MemoryStore`, but *Heroku* [says](https://devcenter.heroku.com/articles/node-sessions#sessions-and-scaling) this is not a best practice in production applications.

*Heroku* [recommends](https://devcenter.heroku.com/articles/node-sessions#storing-sessions-in-redis) *Redis* for session storage, but we don't need to add another dependency to our technology stack. Let's instead use the datastore we chose in the previous post.

### Option A - *PostgreSQL* Session Store

If you chose a *PostgreSQL* datastore, let's configure a new table called `sessions` in our database to hold session information.












### Option B - *MongoDB* Session Store

If you chose a *MongoDB* datastore, let's configure a new collection called `sessions` to hold session information.
