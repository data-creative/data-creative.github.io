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

The **Next Train API** provides a JSON web service for any GTFS feed. Currently this back-end API powers the <a href="{{ site.baseurl }}/open-source-application/2017/06/30/next-train-mobile/">Next Train Mobile App</a> I built for Android.

> If you are curious, [GTFS](https://developers.google.com/transit/gtfs/) stands for "General Transit Feed Specification", the data standard transit agencies use to publish their train schedule data. They are supposed to all publish data in a consistent format to enable a robust and interoperable ecosystem of third-party developer apps.

Data published in GTFS format consists of a ZIP file which includes a number of normalized CSV files. In such a format, it's not easy for client applications to make requests for the data. So I built the API to process and store GTFS data into a database on a nightly or hourly basis, then to respond to on-demand requests with concise JSON formatted data.

Example request URL: `/api/v1/trains.json?date=2017-12-17&origin=BRN&destination=NHV`

> The request URL specifies parameters to denote the origin station, destination station, and date of travel. These parameters are then used in a database query to fetch specific schedule data.

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

> The response includes a list of trips running from the origin station to the destination station on the given day. It even includes a comprehensive list of all stops for each trip. Very usable!

Besides transforming GTFS data into a more usable format for client applications, the real value of this API lies in its scalability. It should work for [any one of these](https://transitfeeds.com/feeds) 500+ GTFS-publishing transit agencies across the globe.

But the API's downside is that it depends on the publishing transit agency to be a dependable and capable partner. Unfortunately, in my experience working with GTFS data [published by](https://www.cttransit.com/about/developers) my local Connecticut transit agency, [Shore Line East](http://www.shorelineeast.com/), I have noticed multiple inconsistencies and idiosyncrasies in their data validation and publishing practices. After bringing these issues to their attention, they have eventually responded with fixes, but along the way they have lost my confidence. It's a shame this specific transit agency is not willing and able to collaborate with third-party developers. I think their riders and their service both suffer as a result.

I encourage any interested developer to [configure and deploy](https://github.com/data-creative/next-train-api/blob/master/DEPLOYING.md) this API to your own Heroku server to consume GTFS data from your own local transit agency. All you have to do is set an environment variable called `GTFS_SOURCE_URL` to point to the GTFS feed URL (e.g. `"http://www.shorelineeast.com/google_transit.zip"`) and schedule a job (`rake gtfs:import`) to run once per hour or once per day. If you do this, let me know how it goes! I'm happy to provide support.
