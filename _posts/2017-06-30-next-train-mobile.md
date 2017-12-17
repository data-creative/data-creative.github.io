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
  - gradle
  - gtfs
  - git
  - native-base
  - node.js
  - npm
---

**NextTrain - Connecticut** is a mobile app for searching train schedules published by the [Shore Line East](http://www.shorelineeast.com/) transit agency in Connecticut.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/next-train-ct-android-demo.gif" alt="">

It relies on a back-end web service I created, called the <a href="{{ site.baseurl }}/open-source-application/2017/05/21/next-train-api/">Next Train API</a>, to provide train schedules in JSON format upon request.

The front-end mobile application is [available to download](https://play.google.com/store/apps/details?id=com.nexttrainct&hl=en) for Android via the Google Play store.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/next-train-ct-android-store.png" alt="">

> This is the first time I have released an Android app to the store!

## Problem Statement

I built this app because I found Shore Line East's online ["Trip Planner" interface](http://www.shorelineeast.com/trip-planner) to be cumbersome and painful. It involves too many clicks (a minimum of seven) and doesn't allow the user to customize his/her preferred stations. Worst of all, unless the user specifies the exact start and end times, when it returns results, it forces the user to scroll through a small modal window to find the relevant information.

There has to be a simpler, easier way!

## Design Principles

When designing the app, I prioritized simplicity. I noticed from my own user behavior that there was a single trip I took all the time, with the same origin and destination station. I assumed commuters would have the same user experience. So I allowed the user to specify a favorite route to minimize future clicking.

Once a user has specified a favorite route, this route shows up automatically when they open the app, and subsequent schedule searches only involve one click!

The best user feedback I have received about this app is that "It looks like it was built by Google". So kudos to [Native Base](https://nativebase.io/) for providing the slick UI components. I would consider using NativeBase in the future, but I also have my eye on [Twitter Bootstrap components](https://react-bootstrap.github.io/introduction.html) making their way into the React Native ecosystem.
