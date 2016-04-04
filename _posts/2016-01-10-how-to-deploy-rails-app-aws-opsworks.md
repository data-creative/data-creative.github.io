---
layout: post
title:  "How to deploy a Ruby on Rails application to AWS EC2 using OpsWorks"
author: MJ Rossetti
categories:
 - posts
 - process-documentation
img: amazon-web-services-logo.png
tags: devops
published: false
icon_class:
technologies:
 - aws
 - ec2
 - opsworks
 - mysql
 - bundler
 - rake
credits:
 - http://levvel.io/blog-post/deploying-rails-4-postgresql-to-aws-opsworks/
 - http://docs.aws.amazon.com/opsworks/latest/userguide/attributes-json-deploy.html#attributes-json-deploy-app-db
 - http://stackoverflow.com/questions/17412279/opsworks-rails-console-environment
 - http://willkoehler.net/2014/10/15/deploy-a-rails-app-with-aws-opsworks.html
 - http://tomoconnor.eu/blogish/how-i-broke-aws-opsworks/
 - https://ruby.awsblog.com/post/Tx7FQMT084INCR/Deploying-Ruby-on-Rails-Applications-to-AWS-OpsWorks
 - http://stackoverflow.com/q/34713738/670433
---


Login to the AWS Management Console.

## Key Pair

Navigate to the EC2 service.
 Navigate to the desired region.  
 Navigate to Network & Security > Key Pairs. Create and download a new Private Key and manage its file permissions.

```` sh
# replace `mykey` with the name of your own private key file
mv ~/Downloads/mykey.pem ~/.ssh
chmod 400 ~/.ssh/mykey.pem
````

## Stack

Navigate to the OpsWorks service.

Add a new Stack:
 + Chef 11
 + name it
 + choose the desired region
 + Amazon Linux
 + choose the private key you just created as the default SSH key

## Layers and Instances

### Database Server

Add a new MySQL Layer. Note the root user password. Add a new instance for the MySQL Layer. Start the new database server instance. Login to the database server.

```` sh
# replace 'mykey' and 'MY.IP.ADDRESS' with your own custom values:
ssh -i ~/.ssh/mykey.pem ec2-user@MY.IP.ADDRESS
mysql -uroot -p # then type the root database user password
````

Improve security by deleting superfluous and unsecured user accounts.

```` sql
DELETE FROM mysql.user WHERE host <> 'localhost' OR USER = "";
````

Create a new database user and password corresponding to the values in the rails application's `database.yml` file.

```` sql
-- replace 'myuser' and 'mypassword' with your own custom values:
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypassword';
GRANT ALL ON *.* to 'myuser'@'localhost';
````

### Application Server

Add a new Rails App Server Layer. Add a new instance for the App Server Layer. Start the app server.

## App

Add a new App:
 + name it
 + Ruby on Rails
 + OpsWorks data source type
 + choose the database instance
 + name the database to match the application's database.yml file
 + specify github repo url
 + add environment variables:
   + `SECRET_KEY_BASE`
   + `MYAPPNAME_DATABASE_PASSWORD`

Deploy the app.

Optionally login to the app server for debugging.

```` sh
# replace 'mykey' and 'MY.IP.ADDRESS' with your own custom values:
ssh -i ~/.ssh/mykey.pem ec2-user@MY.IP.ADDRESS
````

```` sh
sudo su deploy
cd /srv/www/myappname/current
# sudo bundle exec rails console
# cat log/production.log # these are empty?
cat /var/log/httpd/myappname-access.log
cat /var/log/httpd/error.log
````













Ensure the rails application's Gemfile includes a javascript runtime (e.g. `gem 'therubyracer', platforms: :ruby`), and rake (`gem 'rake', '10.4.2'`).










You may need to manually install rake or fix path issues:

```` sh
cd /srv/www/myappname/current
sudo bundle install --path vendor/cache
sudo bundle install --path vendor/bundle
````
