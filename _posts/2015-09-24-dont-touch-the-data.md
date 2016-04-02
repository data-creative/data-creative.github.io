---
layout: post
title:  "Don't Touch the Data"
author: MJ Rossetti
categories:
 - posts
 - best-practices
img: scene-of-crime.jpg
use_img_as_post_header: true
tags: none
published: true
icon_class: none
technologies:
 - ruby
 - python
 - r
credits:
 - https://www.archaeological.org/pdfs/AIATourismGuidelines.pdf
---

<!--
![a photo of one crime scene investigator putting on white gloves while the other takes pictures](http://www.itsgov.com/wp-content/uploads/2014/01/The-Science-Of-Crime-Scene-Forensics-A-Detailed-Overview.jpg)
-->

Data Extraction, Transformation, and Loading (ETL) requires strict adherence to repeatable scientific processes.

When encountering raw data, analysts should assume the mentality of an archeologist or crime scene investigator. Archaeologists avoid improper handling of artifacts, else they risk causing damage and deterioration to the excavation site. Crime scene investigators don't tamper with the evidence, else it may become inadmissible in a court of law.

Likewise, data scientists should avoid performing undocumented manual transformation processes on a dataset, else they diminish its value. Any resulting information findings or interpretations are rendered unreliable.

If manual transformation processes are necessary, data scientists should endeavor to document and review their actions to the fullest extent possible.

Else, data scientists should leverage scripting languages like *Ruby*, *Python*, or *R* to process the data in an accurate, repeatable, programmatic way.
