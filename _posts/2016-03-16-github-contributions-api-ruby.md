---
layout: post
title:  "GitHub Contributions API - Ruby Library"
author: MJ Rossetti
published: true
img: rubygems_logo_red.png
repo_url: https://github.com/data-creative/github-contributions-api-ruby
project_url: https://rubygems.org/gems/github_contributions_api
categories:
 - open-source-library
technologies:
 - ruby
 - rubygems
 - bundler
 - github
 - git
 - json
 - api
 - rspec
---

I wanted to get a comprehensive history of my GitHub commits and store them in a database.

I started using the [Octokit *Ruby* library](https://github.com/octokit/octokit.rb)
  to interface with the [official GitHub API](https://developer.github.com/v3/),
  but it only provides 90 days of event history.

> Only events created within the past 90 days will be included in timelines. Events older than 90 days will not be included (even if the total number of events in the timeline is less than 300). - https://developer.github.com/v3/activity/events/

After searching the Internet, I discovered the [GitHub Archive](https://www.githubarchive.org/), a service which "record[s] the public GitHub timeline, archive[s] it, and make[s] it easily accessible for further analysis."
  The [dataset](https://bigquery.cloud.google.com/table/githubarchive:day.events_20150101?pli=1) is available on Google BigQuery.

I could have written scripts to run against Google BigQuery, but ultimately I found another existing service called the [GitHub Contributions Archive](https://githubcontributions.io/) which displays [comprehensive event history](https://githubcontributions.io/user/s2t2/events/1) from the GitHub Archive for each user.
 One of the most convenient parts of this service is that in addition to including contributions to a user's own repositories, it also includes contributions to repositories not owned by the user.

After [inquiring](https://github.com/tenex/github-contributions/issues/80#issue-141110175), I learned the Contributions Archive makes the underlying data available via *JSON API*.

User Request:

```` sh
curl https://githubcontributions.io/api/user/octocat
````

User Response:

```json
{
  "username":"octocat",
  "repos":[
    "OpenELEC/OpenELEC.tv",
    "octocat/Hello-World",
    "octocat/Spoon-Knife",
    "octocat/ThisIsATest",
    "octocat/git-alliance",
    "octocat/git-consortium",
    "octocat/octocat.github.io"
  ],
  "eventCount":28
}
```

Events Request:

```` sh
curl https://githubcontributions.io/api/user/octocat/events/1
````

Events Response:

```` json
{
  "events":[
    {
      "_event_id":"3342744700",
      "_id":"564c28d972c4347a4234f081",
      "_repo_lower":"octocat/octocat.github.io",
      "_user_lower":"octocat",
      "before":"e2daa5755ee24217f39a10dc6b94988c19e0f109",
      "commit_count":1,
      "created_at":"2015-11-14T22:22:37+00:00",
      "distinct_commit_count":1,
      "head":"3a9796cf19902af0f7e677391b340f1ae4128433",
      "ref":"refs/heads/master",
      "repo":"octocat/octocat.github.io",
      "type":"PushEvent",
      "user":"octocat"
    },
    {}, {}, {}, etc ...
    {
      "_id":"564c3faf72c4347a42cd57d5",
      "_org_lower":"",
      "_repo_lower":"octocat/hello-world",
      "_user_lower":"octocat",
      "before":null,
      "commit_count":null,
      "created_at":"2014-06-10T22:22:28+00:00",
      "distinct_count":null,
      "head":"b3cbd5bbd7e81436d2eee04537ea2b4c0cad4cdf",
      "org":"",
      "ref":"refs/heads/test",
      "repo":"octocat/Hello-World",
      "type":"PushEvent",
      "user":"octocat"
    }
  ]
}
````

To interface with this data using the *Ruby* language, I wrote and published a small library to help facilitate the *HTTP GET* requests.

```` rb
user = GithubContributionsApi.user("octocat")

events = GithubContributionsApi.user_events("octocat")

my_events = GithubContributionsApi.user_events("s2t2", :page => 2)
````

[Star the repo]({{ page.repo_url }}/stargazers) if you find it helpful.
