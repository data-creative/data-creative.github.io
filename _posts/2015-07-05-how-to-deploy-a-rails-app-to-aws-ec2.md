---
layout: post
title:  "How to deploy a Ruby on Rails app to AWS EC2"
author: MJ Rossetti
categories:
 - posts
 - process-documentation
img: amazon-web-services-logo.png
tags: none
published: false
icon_class: none
technologies:
 - ruby
 - chef
 - knife-solo
 - rake
 - rails
 - mysql
 - passenger
---

This document describes the process of deploying an existing Ruby on Rails application to a production server, in this case an AWS EC2 instance.

## Prerequisites

### Source Code

Ensure the source code for your app is version-controlled using git, and hosted on GitHub or BitBucket.

Obtain the hosted repository's SSH clone url.

### Deploy Key

Create a new deploy key in the settings for your repo.

host name | clone url template | deploy key settings url template
--- | --- | ---
GitHub | git@github.com:ORG_NAME/APP_NAME.git | https://github.com/ORG_NAME/APP_NAME/settings/keys
BitBucket | git@bitbucket.org:ORG_NAME/APP_NAME.git | https://bitbucket.org/ORG_NAME/APP_NAME/admin/deploy-keys

### Server

Ensure existance of a fresh AWS EC2 instance. See [how to launch an AWS EC2 instance](process-documentation/2015/07/05/how-to-launch-an-aws-ec2-instance.html) for instructions.
