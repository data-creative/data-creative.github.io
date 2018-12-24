---
layout: post
title:  "MyBanner - Ruby Library"
author: MJ Rossetti
published: true
img: rubygems_logo_red.png
repo_url: https://github.com/prof-rossetti/my-banner-rb
project_url: https://rubygems.org/gems/my_banner
categories:
 - projects
technologies:
 - ruby
 - rubygems
 - bundler
 - git
 - rspec
 - ellucian-banner
 - google-apis
 - google-calendar
 - google-drive
 - google-sheets
---

### Problem Statement

As a university-level instructor, I use [Google Calendar](https://calendar.google.com) and [Google Sheets](https://docs.google.com/spreadsheets) to manage calendars and gradebook files, respectively, for the courses I teach. The process of manually creating these calendars and spreadsheet can be tedious, especially when I'm scheduled to teach many classes. I'd rather automate it, so I can spend my time doing other things.


### Solution

This ruby library contains scripts to process a user's schedule information from the [Ellucian Banner](https://www.ellucian.com/solutions/ellucian-banner) information system to generate calendar events and gradebook files for each scheduled class.

At this point I've only tested the program on faculty schedules, but it should work for student schedules as well. As long as your school uses the Banner information system, this program should work for you too!

[Star the repo]({{ page.repo_url }}/stargazers) if you find it helpful.
