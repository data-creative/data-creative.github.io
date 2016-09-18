---
layout: post
title:  "Creating a CRUD Application with React.js"
author: MJ Rossetti
published: true
img: react-banner-logo.png
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

## Application Architecture and Mechanics

This node.js application includes a back-end API and a front-end interface.

### API

The back-end API uses [express.js](http://expressjs.com/) to serve JSON at `/api` endpoints.

> relevant file: `app.js` and anything in the `routes/` directory

The server was generated using [express-generator](https://expressjs.com/en/starter/generator.html).

#### Datastore

The API serves data from a [MongoDB](https://www.mongodb.com/) datastore, with database connections and data models handled by [mongoose.js](https://github.com/Automattic/mongoose).

> relevant files: `db.js`, `models/robot.js` and anything in the `db/` directory

#### Tests

Testing the API server was straightforward using [mocha.js](https://mochajs.org/) as a generic node.js testing framework, and more specifically [supertest](https://github.com/visionmedia/supertest) for testing HTTP GET and POST requests.

> relevant files: anything in the `test/api/` directory

### Interface

The front-end interface is a [React](https://github.com/facebook/react) application.

> relevant files: anything in the `src/components/` directory

#### Build Process

React components are written in [JSX](https://jsx.github.io/), which in order to be interpreted by browsers requires conversion into normal JavaScript. This build process is run by [webpack](https://github.com/webpack/webpack), which relies on [babel.js](https://babeljs.io/) to translate various flavors of JSX. The build process created a file called `dist/bundle.js`.

> relevant files: `.babelrc`, `webpack.config.js`, `webpack.dev.config.js`

The express server needs to be edited to serve static files (namely the bundle.js) from the `dist/` directory, and to use webpack-related middleware. This includes middleware for the exciting [hot-reloading](https://facebook.github.io/react-native/blog/2016/03/24/introducing-hot-reloading.html) feature which updates react components without need to refresh the page. In development mode, the hot-reload middleware short-cuts the process of creating a build file, instead managing the build in-memory.

#### Routing

The express server recognizes navigation to the root url and serves the react bundle. Navigation between various react components is handled by [react-router](https://github.com/ReactTraining/react-router).
  In development, react router is configured to use "hash" urls (e.g. `http://localhost:3000/#/?_k=hpvxlt`), and visits to unrecognized urls are handled properly (i.e. a visit to `http://localhost:3000/#/robots/my_bot` results in a redirect to the root url with a flash message saying "Couldn't find robot #my_bot".
  In production, react router is configured to use cleaner-looking urls, (e.g. `https://react-robots.herokuapp.com/robots/new`), but unfortunately visits to unrecognized urls result in 404 errors.

> relevant file: `src/entry.js` and the react-related sections of `app.js`

Flash messaging logic is handled by react components, not the express server.

#### Testing

A number of frameworks were investigated as candidates for implementing react component integration tests.

React component tests were implemented using a number of frameworks, with [selenium-webdriver](http://www.seleniumhq.org/projects/webdriver/) emerging as perhaps the most stable, even though it's documentation is old and it intoruces a dependancy on java for running the webdriver server.

> relevant files: anything in the `test/components/` directory

<hr>

[![](/assets/img/posts/express-robots-index.png)](https://react-robots.herokuapp.com/)
