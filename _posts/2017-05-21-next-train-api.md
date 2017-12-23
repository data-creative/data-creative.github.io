---
layout: post
title:  "NextTrain API"
author: MJ Rossetti
published: true
repo_url: https://github.com/data-creative/next-train-api
project_url: http://next-train-production.herokuapp.com/
img: rails-logo.svg
use_img_as_post_header: true
categories:
 - open-source-application
technologies:
  - bundler
  - ruby
  - rails
  - rspec
  - mysql
  - heroku
  - git
  - gtfs
---

The **Next Train API** provides a JSON web service for any General Transit Feed Specification ([GTFS](https://developers.google.com/transit/gtfs/)) feed. By publishing transit schedule data in a consistent format like GTFS, transit agencies enable a robust and interoperable ecosystem of third-party developer applications (e.g Google Maps and <a href="{{ site.baseurl }}/open-source-application/2017/06/30/next-train-mobile/">Next Train Mobile</a>).

## Problem Statement

For performance reasons, it is not practical for client applications to directly consume data in GTFS format. GTFS comprises a comprehensive ZIP file of normalized CSV files, not a concise JSON response to the parameters of a specific request.

## Solution

The NextTrain API acts as an intermediary between the the client application and the raw GTFS data. It is designed to optimize the efficiency of the client application request process.

The system pre-processes GTFS data by checking a specified GTFS feed URL for new data on a scheduled basis and storing the results in a relational database. This facilitates future on-demand querying.

Example request URL: `/api/v1/trains.json?date=2017-12-17&origin=BRN&destination=NHV`

> The request URL specifies parameters which denote the origin station, destination station, and date of travel.

Example response:

```js
{
  "query": {"origin": "BRN", "destination": "NHV", "date": "2017-12-17"},
  "received_at": "2017-12-17 18:00:14 -0500",
  "response_type": "Api::V1::TrainsResponse",
  "errors": [],
  "results": [
    {
      "schedule_id": 22,
      "service_guid": "WE",
      "route_guid": "SLE",
      "trip_headsign": "Westbound",
      "trip_guid": "3613",
      "origin_departure": "2017-12-17T07:24:00-0500",
      "destination_arrival": "2017-12-17T07:40:00-0500",
      "stops": [
        {
          "stop_sequence": 378,
          "stop_guid": "NLC",
          "arrival_time": "2017-12-17T06:30:00-0500",
          "departure_time": "2017-12-17T06:30:00-0500"
        },
        {
          "stop_sequence": 379,
          "stop_guid": "OSB",
          "arrival_time": "2017-12-17T06:55:00-0500",
          "departure_time": "2017-12-17T06:55:00-0500"
        },
        {
          "stop_sequence": 380,
          "stop_guid": "WES",
          "arrival_time": "2017-12-17T07:00:00-0500",
          "departure_time": "2017-12-17T07:00:00-0500"
        },
        {
          "stop_sequence": 381,
          "stop_guid": "CLIN",
          "arrival_time": "2017-12-17T07:05:00-0500",
          "departure_time": "2017-12-17T07:05:00-0500"
        },
        {
          "stop_sequence": 382,
          "stop_guid": "MAD",
          "arrival_time": "2017-12-17T07:10:00-0500",
          "departure_time": "2017-12-17T07:10:00-0500"
        },
        {
          "stop_sequence": 383,
          "stop_guid": "GUIL",
          "arrival_time": "2017-12-17T07:16:00-0500",
          "departure_time": "2017-12-17T07:16:00-0500"
        },
        {
          "stop_sequence": 384,
          "stop_guid": "BRN",
          "arrival_time": "2017-12-17T07:24:00-0500",
          "departure_time": "2017-12-17T07:24:00-0500"
        },
        {
          "stop_sequence": 385,
          "stop_guid": "ST",
          "arrival_time": "2017-12-17T07:37:00-0500",
          "departure_time": "2017-12-17T07:37:00-0500"
        },
        {
          "stop_sequence": 386,
          "stop_guid": "NHV",
          "arrival_time": "2017-12-17T07:40:00-0500",
          "departure_time": "2017-12-17T07:40:00-0500"
        }
      ]
    },
    // ... etc. ...
    {
      "schedule_id": 22,
      "service_guid": "WE",
      "route_guid": "SLE",
      "trip_headsign": "Westbound",
      "trip_guid": "3621",
      "origin_departure": "2017-12-17T09:19:00-0500",
      "destination_arrival": "2017-12-17T09:35:00-0500",
      "stops": [
        {
          "stop_sequence": 402,
          "stop_guid": "NLC",
          "arrival_time": "2017-12-17T08:25:00-0500",
          "departure_time": "2017-12-17T08:25:00-0500"
        },
        {
          "stop_sequence": 403,
          "stop_guid": "OSB",
          "arrival_time": "2017-12-17T08:50:00-0500",
          "departure_time": "2017-12-17T08:50:00-0500"
        },
        {
          "stop_sequence": 404,
          "stop_guid": "WES",
          "arrival_time": "2017-12-17T08:55:00-0500",
          "departure_time": "2017-12-17T08:55:00-0500"
        },
        {
          "stop_sequence": 405,
          "stop_guid": "CLIN",
          "arrival_time": "2017-12-17T09:00:00-0500",
          "departure_time": "2017-12-17T09:00:00-0500"
        },
        {
          "stop_sequence": 406,
          "stop_guid": "MAD",
          "arrival_time": "2017-12-17T09:05:00-0500",
          "departure_time": "2017-12-17T09:05:00-0500"
        },
        {
          "stop_sequence": 407,
          "stop_guid": "GUIL",
          "arrival_time": "2017-12-17T09:11:00-0500",
          "departure_time": "2017-12-17T09:11:00-0500"
        },
        {
          "stop_sequence": 408,
          "stop_guid": "BRN",
          "arrival_time": "2017-12-17T09:19:00-0500",
          "departure_time": "2017-12-17T09:19:00-0500"
        },
        {
          "stop_sequence": 409,
          "stop_guid": "ST",
          "arrival_time": "2017-12-17T09:32:00-0500",
          "departure_time": "2017-12-17T09:32:00-0500"
        },
        {
          "stop_sequence": 410,
          "stop_guid": "NHV",
          "arrival_time": "2017-12-17T09:35:00-0500",
          "departure_time": "2017-12-17T09:35:00-0500"
        }
      ]
    }
  ]
}
```

> The response includes a list of trips running from the origin station to the destination station on the given day. It even includes a comprehensive list of all stops for each trip.

### Collaboration

Any product which consumes GTFS data depends on the respective transit agency to publish quality data. Unfortunately, when working with data [published by](https://www.cttransit.com/about/developers) my local transit agency, [Shore Line East](http://www.shorelineeast.com/), I noticed chronic data integrity issues and publishing process inconsistencies. Even though the agency was somewhat responsive when I brought these issues to their attention, I lost confidence in the agency's ability to consistently deliver quality data.

To mitigate these issues in the future, transit agencies like Shore Line East should ["eat their own dog food"](https://www.investopedia.com/terms/e/eatyourowndogfood.asp). In other words, an agency's own services (e.g. Shore Line East's [Trip Planner](http://www.shorelineeast.com/trip-planner)) should consume the agency's own GTFS data. By becoming its own first customer, an agency increases its reliance on its own data. This reliance puts the right incentive structure in place for the agency to prioritize the quality of its data.

### Adaptability

Luckily, this specific agency isn't the only publisher of GTFS data. The NextTrain API should work for any one of these [500+ GTFS-publishing transit agencies](https://transitfeeds.com/feeds) across the globe.

I encourage any interested developer to [configure and re-deploy](https://github.com/data-creative/next-train-api/blob/master/DEPLOYING.md) this API to consume GTFS data from any of these other transit agencies. Configuration involves setting an environment variable called `GTFS_SOURCE_URL` to point to the desired GTFS feed URL, and scheduling the server to run a job (`rake gtfs:import`) on a recurring basis. If you do this, let me know how it goes! I'm happy to provide support.
