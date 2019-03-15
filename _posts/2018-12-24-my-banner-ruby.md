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
 - travis-ci
---

### Problem Statement

As a university-level instructor, I use [Google Calendar](https://calendar.google.com) and [Google Sheets](https://docs.google.com/spreadsheets) to manage calendars and gradebook files, respectively, for the courses I teach. The process of manually creating these calendars and spreadsheet can be tedious, especially when I'm scheduled to teach many classes. I'd rather automate it, so I can spend my time doing other things.


### Solution

The "MyBanner" ruby library contains scripts to process a user's schedule information from the [Ellucian Banner](https://www.ellucian.com/solutions/ellucian-banner) information system to generate calendar events and gradebook files for each scheduled class.

I didn't see any APIs available, so this program gets the data by parsing one of the system's HTML schedule pages into a more structured format.

Example schedule page:

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/my-banner-schedule-html.png" alt="">

Example parsed output:

```rb
{
  :title=>"Intro to Programming",
  :crn=>123456,
  :course=>"INFO 101",
  :section=>20,
  :status=>"OPEN",
  :registration=>"May 01, 2018 - Nov 02, 2018",
  :college=>"School of Business and Technology",
  :department=>"Information Systems",
  :part_of_term=>"C04",
  :credits=>1.5,
  :levels=>["Graduate", "Juris Doctor", "Undergraduate"],
  :campus=>"Main Campus",
  :override=>"No",
  :enrollment_counts=>{:maximum=>50, :actual=>45, :remaining=>5},
  :scheduled_meeting_times=>{
    :type=>"Lecture",
    :time=>"11:00 am - 12:20 pm",
    :days=>"TR",
    :where=>"Science Building 111",
    :date_range=>"Oct 29, 2018 - Dec 18, 2018",
    :schedule_type=>"Lecture",
    :instructors=>["Polly Professor"]
  }
}
```

After parsing the schedule data into this more usable format, the program is able to automatically compile a list of meetings for each class and create calendar events for each meeting.

At this point I've only tested the program on faculty schedules, but it should work for student schedules as well. As long as your school uses the Banner information system, this program should work for you too. Try it out, and [star the repo]({{ page.repo_url }}/stargazers) if you find it helpful.
