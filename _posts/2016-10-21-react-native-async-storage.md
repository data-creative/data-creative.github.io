---
layout: post
title:  "Store and retrieve data from Android local filesystem using React-Native AsyncStorage"
author: MJ Rossetti
published: false
img: react-native-logo.png
categories:
 - process-documentation
technologies:
  - node.js
  - npm
  - react.js
  - react-native
  - android
  - java
  - android-studio
  - nosql
  - mongodb
---

`AsyncStorage` is awesome. Its as if the `MongoDB` NoSQL datastore came pre-installed on Android devices.

But if you're going to use it, make sure to `JSON.stringify()` before storing and `JSON.parse()` what you retrieve. Or you might run into errors like `_______`.

The [`react-native-simple-store`](https://github.com/jasonmerino/react-native-simple-store) package [illustrates](https://github.com/jasonmerino/react-native-simple-store/blob/2bf2d3797370c2ce92e9958165969d2db9ef4fa5/dist/index.js#L24-L63) how to use `AsyncStorage`. Excerpts below:


```` js

import {AsyncStorage} from 'react-native';

const myVal = {}

AsyncStorage.setItem("myKey", JSON.stringify(myVal)).then(function(){
  console.log('AsyncStorage setItem() Success', myVal)
  // set state here
}).catch(function(error){  console.log('AsyncStorage setItem() Error:', error) })
````

These calls to the local filesystem require logic to handle asynchronous responses. They return Promises and can be chained:

```` js
````

Have fun with `AsyncStorage`.
