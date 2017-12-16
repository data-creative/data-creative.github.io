---
layout: post
title:  "Creating a CRUD Application with React.js"
author: MJ Rossetti
published: true
img: react-robots-homepage.png # react-logo.png
categories:
 - open-source-application
repo_url: https://github.com/s2t2/react-robots
project_url: https://react-robots.herokuapp.com/
technologies:
  - git
  - node.js
  - npm
  - express.js
  - twitter-bootstrap
  - mongodb
  - react.js
  - webpack
  - mocha
  - selenium-webdriver
  - heroku
---

[React Robots](https://react-robots.herokuapp.com/) is a node.js CRUD application, including a back-end API and a front-end interface.

<!--
![a screenshot of a web app depicting a table consisting of three "robot" records. it has buttons for editing and deleting records, as well as for creating new records and "recycling" the database state](/assets/img/posts/react-robots-homepage.png)
-->

### API

The back-end API uses [express.js](http://expressjs.com/) to serve JSON at `/api` endpoints.

> reference: `app.js`, `routes/`

The server was generated using [express-generator](https://expressjs.com/en/starter/generator.html).

#### Datastore

The API serves data from a [MongoDB](https://www.mongodb.com/) datastore, with database connections and data models handled by [mongoose.js](https://github.com/Automattic/mongoose).

> reference: `db.js`, `models/robot.js`, `db/`

#### Tests

Testing the API server was straightforward using [mocha.js](https://mochajs.org/) as a generic node.js testing framework, and more specifically [supertest](https://github.com/visionmedia/supertest) for testing HTTP GET and POST requests.

> reference: `test/api/`

### Interface

The front-end interface is a [React](https://github.com/facebook/react) application.

> reference: `src/components/`

#### Build Process

React components are written in [JSX](https://jsx.github.io/), which in order to be interpreted by browsers requires conversion into normal *JavaScript*.

This build process is performed by [webpack](https://github.com/webpack/webpack), which relies on [babel.js](https://babeljs.io/) to translate various flavors of JSX.

> reference: `.babelrc`, `webpack.config.js`, `webpack.dev.config.js`

In production, the build process creates a file called `dist/bundle.js`, which the server is configured to recognize. In development mode, the bundle is managed in-memory by [webpack-dev-middleware](https://github.com/webpack/webpack-dev-middleware).

In development, [webpack-hot-middleware](https://github.com/glenjamin/webpack-hot-middleware) enables react's [hot-reloading](https://facebook.github.io/react-native/blog/2016/03/24/introducing-hot-reloading.html) feature, which updates components without need to refresh the page.

#### Routing

The express server recognizes navigation to the root url and serves the react bundle. Navigation between various react components is handled by [react-router](https://github.com/ReactTraining/react-router).
  In development, react router is configured to use "hash" urls (e.g. `http://localhost:3000/#/?_k=hpvxlt`), and visits to unrecognized urls are handled properly (i.e. a visit to `http://localhost:3000/#/robots/my_bot` results in a redirect to the root url with a flash message saying "Couldn't find robot #my_bot".
  In production, react router is configured to use cleaner-looking urls, (e.g. `https://react-robots.herokuapp.com/robots/new`), but unfortunately visits to unrecognized urls result in 404 errors.

> reference: `src/entry.js`, react-related sections of `app.js`

Flash messaging logic is handled by react components, not the express server.

#### Testing

Integration tests for react components required investigation into various browser testing frameworks. Perhaps the most stable is [selenium-webdriver](http://www.seleniumhq.org/projects/webdriver/), although it depends on java to run a separate webdriver server.

> reference: `test/components/`
