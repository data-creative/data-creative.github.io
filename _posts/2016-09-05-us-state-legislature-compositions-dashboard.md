---
layout: post
title:  "US State Legislature Compositions"
author: MJ Rossetti
categories:
 - projects
img: state-legislature-compositions.png
repo_url: https://github.com/data-creative/us-state-legislature-compositions
project_url: http://data-creative.info/us-state-legislature-compositions/
use_img_as_post_header: true
tags: data-visualization map ncsl legislature
published: true
icon_class: frown-o
technologies:
 - html
 - css
 - javascript
 - d3.js
 - mapbox.js
 - git
 - json
 - pdf
 - csv
 - twitter-bootstrap
 - ruby
 - pdftotext
 - geojson
---

## Background

The [National Council of State Legislatures (NCSL)](http://www.ncsl.org/) provides information on its website about the gender and party compositions of state legislatures from 2009 to 2016.

The source data was available in PDF and HTML formats, so it required additional processing before becoming usable.

## Data Engineering

Seeking machine-readable versions of the NCSL data, I found an [open source GitHub repository](https://github.com/dwillis/state_legislatures), but it hadn't been updated in a few years. Happy to contribute to an open source project, I sought to update the repository with the most recent years' data.

Through a Twitter conversation with the repository owner, I learned the existing conversion process involved software called Tabula. When I used Tabula to convert the most recent two years of party composition data, I ran into issues.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/derekwillis">@derekwillis</a> <a href="https://twitter.com/NCSLorg">@NCSLorg</a> not very well, at least with tabula - they end up looking like this: <a href="https://t.co/Epc9uFKfv4">https://t.co/Epc9uFKfv4</a> <a href="https://t.co/DraBGzcT8v">pic.twitter.com/DraBGzcT8v</a></p>&mdash; MJ Rossetti (@s2t2) <a href="https://twitter.com/s2t2/status/765246060742766593">August 15, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

In response to these pdf-conversion issues, the owner suggested the `pdftotext` command line utility, which ultimately produced adequate TXT files.

<blockquote class="twitter-tweet" data-conversation="none" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/s2t2">@s2t2</a> <a href="https://twitter.com/NCSLorg">@NCSLorg</a> looks much better using pdftotext -layout <a href="https://t.co/Lvl98WUZ8c">pic.twitter.com/Lvl98WUZ8c</a></p>&mdash; Derek Willis (@derekwillis) <a href="https://twitter.com/derekwillis/status/765250069109043200">August 15, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

After writing a Ruby script leveraging the `pdftotext` library to convert the PDF files to TXT format, I wrote scripts to convert the TXT files to CSV and JSON formats. I then wrote scripts to convert the gender composition HTML tables to CSV and JSON formats.

After initial satisfaction with the conversion results, I introduced validation checks into the process. Theses validations uncovered a [few](https://github.com/AdvancedEnergyEconomy/state_legislatures/issues/3) [errors](https://github.com/AdvancedEnergyEconomy/state_legislatures/issues/9) in the source data, which I communicated to NCSL via Twitter and remediated by updating the conversion scripts.

When satisfied with the validation effort, I submitted a pull request to add the most recent data and the automated conversion scripts to the original repository.

## Software Engineering

After producing a full compliment of machine-readable data, I created an [interactive dashboard](http://data-creative.info/us-state-legislature-compositions/) to consume the data and aid in exploration.

## Data Analysis

The sections below contain findings from my analysis of 2016 NCSL Legislature Composition data.

### Legislature Sizes

Despite its small size, New Hampshire's legislature is the largest (424 seats), while the 13-seat [DC City Council](http://dccouncil.us/council) is the smallest.

![a greyscale choropleth map](/assets/img/posts/state-legislature-sizes.png)

### Legislature Gender Compositions

Colorado and Vermont legislatures have the highest concentration of females (each over 40%).

![a pink-scale choropleth map](/assets/img/posts/state-legislature-pct-female.png)

Legislatures from Wyoming, Oklahoma, South Carolina, West Virginia, Alabama, and Mississippi have the highest concentration of males (each over 85%).

![a blue-green-scale choropleth map](/assets/img/posts/state-legislature-pct-male.png)

### Legislature Partisan Compositions

Nebraska's Legislature is [nonpartisan](https://en.wikipedia.org/wiki/Nebraska_Legislature#Selection.2C_composition_and_operation).

The state legislatures with the greatest concentration of Democrat Party members are Hawaii, District of Columbia, and Rhode Island.

![a blue-scale choropleth map](/assets/img/posts/state-legislature-pct-dem.png)

The state legislatures with the greatest concentration of Republican Party members are Wyoming, Utah, and South Dakota.

![a red-scale choropleth map](/assets/img/posts/state-legislature-pct-gop.png)
