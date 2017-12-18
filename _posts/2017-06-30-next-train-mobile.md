---
layout: post
title:  "NextTrain Mobile"
author: MJ Rossetti
published: true
repo_url: https://github.com/data-creative/NextTrainCT
project_url: https://play.google.com/store/apps/details?id=com.nexttrainct&hl=en
img: react-native-logo.png
use_img_as_post_header: true
categories:
 - open-source-application
technologies:
  - javascript
  - react-native
  - android
  - android-studio
  - gradle
  - gtfs
  - git
  - native-base
  - node.js
  - npm
  - moment.js
  - d3.js
---

**NextTrain - Connecticut** is a mobile app for searching train schedules published by the [Shore Line East](http://www.shorelineeast.com/) transit agency in Connecticut.

It relies on a back-end web service I created, called the <a href="{{ site.baseurl }}/open-source-application/2017/05/21/next-train-api/">Next Train API</a>, to provide train schedules in JSON format upon request.

Demo:

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/next-train-ct-android-demo.gif" alt="">

The front-end mobile application is [available to download](https://play.google.com/store/apps/details?id=com.nexttrainct&hl=en) for Android via the Google Play store.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/next-train-ct-android-store.png" alt="">

> This is the first time I have released an Android app to the store!

## Problem Statement

I built this app because I found Shore Line East's online ["Trip Planner" interface](http://www.shorelineeast.com/trip-planner) to be cumbersome and painful. It involves too many clicks (a minimum of seven) and doesn't allow the user to customize his/her preferred stations. Worst of all, unless the user specifies the exact start and end times, when it returns results, it forces the user to scroll through a small modal window to find the relevant information.

There has to be a simpler, easier way!

## Design Principles

When designing the app, I prioritized simplicity of user experience. Specifically, my goal was to minimize the number of clicks required to perform a schedule search.

I noticed from my own user behavior that I was taking the same one or two trips over and over. I assumed commuters would have the same user experience. So I allowed the user to specify favorite routes to minimize future clicking. Once a user has specified a favorite route, the route shows up automatically when they open the app, and subsequent schedule searches only involve one click!

I also noticed I usually searched for today's schedule or tomorrow's schedule, so I hard-coded these favorite searches into the user interface, while at the same time allowing the user to optionally search a custom date.

The best user feedback I have received about this app is that "It looks like it was built by Google". So kudos to [Native Base](https://nativebase.io/) and [React Native Vector Icons](https://github.com/oblador/react-native-vector-icons) for providing the slick UI components and icons, respectively. Also thanks to the [React Native Date-picker](https://github.com/xgfe/react-native-datepicker) and [React Native Swipe List](https://github.com/jemise111/react-native-swipe-list-view) libraries for providing intuitive ways to pick dates and delete list items, respectively.

> Note: I would consider using NativeBase components in the future, but I also have my eye on [Twitter Bootstrap components](https://react-bootstrap.github.io/introduction.html) making their way into the React Native ecosystem. Let's see how the competition unfolds.

## Development Notes

While developing this Android app, I [learned](https://github.com/jasonmerino/react-native-simple-store/blob/2bf2d3797370c2ce92e9958165969d2db9ef4fa5/dist/index.js#L24-L63) how to use React Native's `AsyncStorage` functionality, which accesses a MongoDB-like NoSQL datastore that comes pre-installed on Android devices. Specifically, I used it to store a user's favorite transit routes so they wouldn't disappear after the user closes the app.

These calls to the local filesystem require logic to handle asynchronous responses. They return [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) and can be chained. To avoid JSON parsing errors, make sure to use `JSON.stringify()` before storing the object and `JSON.parse()` after retrieving it.

Relevant code snippets:

  + [Saving data](https://github.com/data-creative/NextTrainCT/blob/231a66217a135fa4c7a1230de5e21ab3c6652fb4/components/favs/New.js#L75-L81) to local storage
  + [Retrieving data](https://github.com/data-creative/NextTrainCT/blob/231a66217a135fa4c7a1230de5e21ab3c6652fb4/components/favs/Index.js#L61-L72) from local storage
  + [Deleting data](https://github.com/data-creative/NextTrainCT/blob/231a66217a135fa4c7a1230de5e21ab3c6652fb4/components/favs/Index.js#L74-L83) from local storage
