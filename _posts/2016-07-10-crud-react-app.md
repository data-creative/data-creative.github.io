---
layout: post
title:  "Creating a CRUD Application with React.js"
author: MJ Rossetti
published: true
img: react-logo.png
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

**[React Robots](https://react-robots.herokuapp.com/)** is a full-stack CRUD application, made in Node.js with MongoDB, Express.js, and React.js (MERN Stack).

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/react-robots-homepage.png" alt="">

It features a back-end API and a front-end interface.

### API

The back-end API uses [Express.js](http://expressjs.com/) to serve JSON at `/api` endpoints.

> Reference: `app.js` and `routes/`.

The API server was generated using [express-generator](https://expressjs.com/en/starter/generator.html).

#### Datastore

The API serves data from a [MongoDB](https://www.mongodb.com/) datastore, with database connections and data models handled by [Mongoose.js](https://github.com/Automattic/mongoose).

> Reference: `db.js`, `models/robot.js`, and `db/`.

#### Tests

Testing the API server was straightforward using [Mocha.js](https://mochajs.org/) as a generic Node.js testing framework, and more specifically [Supertest](https://github.com/visionmedia/supertest) for testing HTTP GET and POST requests.

> Reference: `test/api/`.

### Interface

The front-end interface is a [React.js](https://github.com/facebook/react) application.

> Reference: `src/components/`.

#### Build Process

React components are written in [JSX](https://jsx.github.io/), which in order to be interpreted by browsers requires conversion into normal JavaScript.

This build process is performed by [Webpack](https://github.com/webpack/webpack), which relies on [Babel.js](https://babeljs.io/) to translate various flavors of JSX.

> Reference: `.babelrc`, `webpack.config.js`, and `webpack.dev.config.js`.

In production, the build process creates a file called `dist/bundle.js`, which the server is configured to recognize. In development mode, the bundle is managed in-memory by [Webpack Dev Middleware](https://github.com/webpack/webpack-dev-middleware).

In development, [Webpack Hot Middleware](https://github.com/glenjamin/webpack-hot-middleware) enables React's [hot-reloading](https://facebook.github.io/react-native/blog/2016/03/24/introducing-hot-reloading.html) feature, which updates components without need to refresh the page.

#### Routing

The express server recognizes navigation to the root url and serves the React bundle. Navigation between various React components is handled by [React Router](https://github.com/ReactTraining/react-router).
  In development, React Router is configured to use "hash" urls (e.g. `http://localhost:3000/#/?_k=hpvxlt`), and visits to unrecognized urls are handled properly (i.e. a visit to `http://localhost:3000/#/robots/my_bot` results in a redirect to the root url with a flash message saying "Couldn't find robot #my_bot").
  In production, React Router is configured to use cleaner-looking urls, (e.g. `https://react-robots.herokuapp.com/robots/new`), but unfortunately visits to unrecognized urls result in 404 errors.

> Reference: `src/entry.js`, react-related sections of `app.js`.

Flash messaging logic is handled by React components, not the Express server.

#### Testing

Integration testing React components required investigation into various browser testing frameworks. Perhaps the most stable is [Selenium Webdriver](http://www.seleniumhq.org/projects/webdriver/), although it depends on Java to run a separate webdriver.

> Reference: `test/components/`.
