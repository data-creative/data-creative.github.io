---
layout: post
title:  "How to set up a Mac OS-X development environment"
author: MJ Rossetti
categories:
 - posts
 - process-documentation
img: macbook-air-side.png
use_img_as_post_header: true
tags: none
published: true
icon_class: none
technologies:
 - mac os-x
 - chrome
 - atom
 - homebrew
 - ruby
 - bundler
 - python
 - pip
 - node.js
 - lunchy
 - postgresql
 - mysql
 - mongodb
credits:
  - http://octopress.org/docs/setup/rbenv/
  - https://help.github.com/articles/associating-text-editors-with-git/#using-atom-as-your-editor
  - http://stackoverflow.com/a/28142874/670433
  - http://www.moncefbelyamani.com/how-to-install-postgresql-on-a-mac-with-homebrew-and-lunchy/
  - http://blog.willj.net/2011/05/31/setting-up-postgresql-for-ruby-on-rails-development-on-os-x/
  - http://stackoverflow.com/a/17936043/670433
  - http://www.postgresql.org/docs/current/interactive/app-psql.html
  - http://www.postgresql.org/docs/7.0/static/security.htm
  - http://www.cyberciti.biz/faq/howto-add-postgresql-user-account/
  - http://dotwell.io/taking-advantage-of-bower-in-your-rails-4-app/
  - https://dev.mysql.com/doc/refman/5.0/en/set-password.html
  - http://blog.bmannconsulting.com/mavericks-brew-cask
  - https://github.com/nicolashery/mac-dev-setup#install
  - https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup
  - http://blog.bmannconsulting.com/mavericks-brew-cask
  - https://github.com/nicolashery/mac-dev-setup#install
  - https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup
  - http://stackoverflow.com/questions/5364340/does-xcode-4-install-git
  - http://blog.grayghostvisuals.com/git/how-to-keep-git-updated/
  - http://lifehacker.com/343328/create-a-keyboard-shortcut-for-any-menu-action-in-any-program
  - http://postgresapp.com/documentation/gui-tools.html
  - https://docs.mongodb.org/v2.4/tutorial/install-mongodb-on-os-x/
  - https://docs.mongodb.org/manual/mongo/
---

This document describes the process of configuring a new Mac OS-X development environment from scratch.

## System Users

In `System Preferences > Users and Groups`, create a new admin user. Restart your computer and login as the new user. Delete the old user and any corresponding home folders.

## System Preferences

### Encryption

In `System Preferences > Security and Privacy > FileVault`, turn FileVault ON. Take a screenshot of the recovery code. Restart the computer when prompted. Monitor progress for the next 20 minutes.

### Keyboard Settings

In `System Preferences > Keyboard`:

  + Repeat: fastest
  + Delay until repeat: shortest
  + Shortcuts:
    * Move left a space: option + left arrow
    * Move right a space: option + right arrow

### Spaces and Hot Corners

In `System Preferences > Mission Control`:

  + Do not auto-arrange.
  + Hot Corners:
    * TL: Mission Control
    * TR: Desktop
    * BR: N/A
    * BL: N/A

Open the Mission Control application and create four new linear horizontal Spaces.

### Dock

Turn Dock hiding on, maximize the size of the Dock, and remove all unnecessary programs from the Dock.

## Terminal

[Download](http://ethanschoonover.com/solarized/files/solarized.zip) the [Solarized](http://ethanschoonover.com/solarized) color themes, and unzip.

In Terminal Settings, import a new Profile, and choose the **solaraized/osx-terminal.app-colors-solarized/Solarized Dark ansi.terminal** theme.

Set the Solarized Dark profile theme as default.

Increase font size to 18.

Restore ~/.bash_profile:

```` sh
# TERMINAL SHORTCUTS AND SETTINGS
alias ll="ls -lahG"
alias gs="git status"
alias gd="git diff"
alias gl="git log"
alias glt="git log --graph --decorate --oneline --full-history --all --simplify-by-decoration"
alias glsd="git ls-files --deleted"

export PS1=" --->> "
export CLICOLOR=1
export LSCOLORS=GxFxCxDxBxegedabagaced
export PATH=/usr/local/bin:$PATH # ADDED BY HOMEBREW
export RBENV_ROOT=/usr/local/var/rbenv # ADDED BY HOMEBREW FOR RBENV
if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi # ADDED FOR RBENV

````

## Browser

Install and/or open [Google Chrome](https://www.google.com/chrome/browser/desktop/index.html), and set it as the default browser.

Sign-in as an existing chrome user via chrome://settings/.

Re-configure Ghostery, or any other plugins as necessary.

## XCode

Create a new [Apple ID](https://appleid.apple.com), and verify your email.

Download [Xcode](https://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12). It might take 30 minutes. View progress from the Launchpad app.

## Homebrew Package Manager

Install the [Homebrew](http://brew.sh/) package manager.

```` sh
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
````

Revise your path to include Homebrew directories.

```` sh
export PATH=/usr/local/bin:$PATH
````

Install [Homebrew Cask](http://caskroom.io/) for downloading native applications.

```` sh
brew tap caskroom/cask
````

## Atom Text Editor

Install the [Atom](https://atom.io/) text editor.

```` sh
brew cask install atom
````

Install [sync-settings](https://github.com/Hackafe/atom-sync-settings).

```` sh
apm install sync-settings
````

In the package settings, specify your github access token and gist id. You can generate a new github access token if you've lost access to the old one. Click "restore" to restore text editor settings.

## Git

Install Git.

```` sh
brew install git
````

Configure [Git credentials](https://help.github.com/categories/setup/):

```` sh
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
git config --global core.editor atom
````

Ensure this email has been added to your GitHub profile and has been verified.


## SSH Keys

Generate [new ssh keys](https://help.github.com/articles/generating-ssh-keys/#step-2-generate-a-new-ssh-key).

```` sh
ssh-keygen -t rsa -b 4096 -C johndoe@example.com # generate new key pair
eval "$(ssh-agent -s)" # start the ssh-agent in the background
ssh-add ~/.ssh/id_rsa # add to keychain
````

Copy the public key to GitHub and other hosts.

```` sh
pbcopy < ~/.ssh/id_rsa.pub # copy to clipboard
````

## Dotfiles

Use the [`s3_sync`](https://github.com/s2t2/s3-sync-ruby) ruby gem to recover files from s3. This requires you to obtain the configuration variables used during previous syncs.

```` sh
gem install "s3_sync"
````

```` rb
require "s3_sync"

S3Sync.configure do |config|
  config.key_id = ENV["AWS_S3_KEY_ID"]
  config.key_secret = ENV["AWS_S3_KEY_SECRET"]
  config.region = ENV["AWS_S3_REGION"]
  config.bucket = ENV["AWS_S3_BUCKET"]
  config.secret_phrase = ENV["AWS_S3_ENCRYPTION_PHRASE"]
  config.files = [
    File.join(Dir.home,".bash_profile"),
    File.join(Dir.home,".ssh","config")
  ]
end

S3Sync.download.new
````

## Rubygems Credentials

Configure [Rubygems credentials](https://rubygems.org/profile/edit), typing your password when prompted:

```` sh
# replace my_rubygems_username with your own username:
mkdir ~/.gem
curl -u my_rubygems_username https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials
````





## Languages and Frameworks

### Ruby

As a developer working on more than one ruby project, it sometimes becomes necessary to specify different ruby versions for each project. Rbenv makes switching ruby versions easy. Ruby-build facilitates the installation of ruby versions.

```` sh
brew install rbenv
````

Follow any post-installation instructions:

 + To use Homebrew's directories rather than ~/.rbenv add to your profile: `export RBENV_ROOT=/usr/local/var/rbenv`
 + To enable shims and autocompletion, run `rbenv init` and follow the instructions to add to your profile: `eval "$(rbenv init -)"`

Restart your terminal for the profile changes to take place.

```` sh
rbenv install 2.2.0 # to install a specific ruby version from the internet
rbenv rehash # run this after installing a new ruby version
rbenv global 2.2.0 # to set a specific ruby version for use
gem install bundler
gem install rake --version='10.4.2'
gem install rdoc -v '4.2.0'
````

### Python

```` sh
brew install python
brew linkapps python
````

### Node

Install [node.js](https://nodejs.org/en/).
  This also installs the [node package manager (npm)](https://www.npmjs.com/).

```` sh
brew install node
````

Install global npm modules.

```` sh
npm install express-generator -g # enables app-generation CLI
npm install nodemon -g # obviates need to restart dev server every time a file is changed
npm install -g node-inspector # allows you to insert debugging breakpoints
npm install -g knex # enables a database migration CLI
````

### Bower

```` sh
cd my_app/
npm install -g bower
# /usr/local/bin/bower -> /usr/local/lib/node_modules/bower/bin/bower
# bower@1.4.1 /usr/local/lib/node_modules/bower
````











## Databases

Use [`lunchy`](https://github.com/eddiezane/lunchy) gem to manage services/daemons, including database servers.

```` sh
gem install lunchy
````

Restart your terminal.

### PostgreSQL

Install.

```` sh
brew install postgresql
ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents # to have launchd start postgresql at login
lunchy start postgresql
createdb # to create a database named after your root database user, which is named after your mac username; avoids `psql: FATAL:  database my_db_user does not exist`
````

Set a password for the database root user.

````
psql -U my_db_user -c "ALTER USER my_db_user WITH PASSWORD 'CHANGE_ME';"
````

To require password authentication, find the location of the *pg_hba.conf* file using `psql -U my_db_user -c "SHOW hba_file;"`, and edit it to resemble to the following template:

    # TYPE  DATABASE        USER            ADDRESS                 METHOD
    local   all             all                                     md5
    host    all             all             127.0.0.1/32            md5
    host    all             all             ::1/128                 md5

Basically you are just changing the *methods* from `trust` to `md5`. Restart the server to apply the authentication changes.

```` sh
lunchy restart postgresql
psql -U my_db_user # you should now be prompted for a password
````

Helpful psql commands and their SQL equivalents:

```` sql
SELECT * FROM pg_user;
-- or... `\du`

SELECT * FROM pg_database;
-- or... `\l`

SELECT * FROM pg_table WHERE schema_name = 'my_db';
-- or... `\connect my_db` and `\dt`
````

Optionally install the pg gem/driver if you are going to be connecting with a Ruby on Rails app. Find the *pg_config* file with `psql -U my_db_user -c "SHOW config_file;"`.

```` sh
gem install pg
# if there is an error and you need to specify the config file: gem install pg -- --with-pg-config=/usr/local/bin/pg_config
````

Optionally create an application database and database user.

```` rb
CREATE USER app_user WITH password 'CHANGE_ME';
ALTER USER app_user CREATEDB;
ALTER USER app_user WITH SUPERUSER;
CREATE DATABASE app_db;
GRANT ALL PRIVILEGES ON DATABASE app_db to app_user;
````

Finally, install [pgAdmin](http://www.pgadmin.org/download/macosx.php) or [pSequel](http://www.psequel.com/) database management software. Specify your root database user credentials when connecting to the local database server.

```` sh
brew cask install psequel
````

### MySQL

Install.

```` sh
brew install mysql
ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
lunchy start mysql
mysql -uroot
````

Secure the connection.

```` sql
DELETE FROM mysql.user WHERE host <> 'localhost' OR USER = "";
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('cleartext password');
````

Helpful mysql commands:

```` sql
SELECT * FROM mysql.user;
SELECT distinct table_schema FROM information_schema.tables; -- or `SHOW DATABASES;`
SELECT * FROM information_schema.tables;
SELECT * FROM information_schema.tables WHERE table_schema = 'my_db';
````

Finally, install [Sequel Pro](http://www.sequelpro.com/download) database management software. Specify your root database user credentials when connecting to the local database server.

```` sh
brew cask install sequel-pro
````



### Mongodb

Install [mongodb](https://www.mongodb.org/).

```` sh
brew install mongodb
````

Follow post-installation instructions.

```` sh
ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
lunchy start mongodb
````

Run mongo.

```` sh
mongo
````

FYI - the mongo config file specifies the expected paths to the mongo data directory and logfile.

```` sh
cat /usr/local/etc/mongod.conf
````

Helpful mongo shell commands:

````
db # to show the active database
show dbs # to show all databases (this doesn't work?)
use myNewDatabase # create/use a new database
show collections # list all collections in the current database (alternate)
db.getCollectionNames() # list all collections in the current database
db.myCollection.insert( { x: 1 } ); # insert a new record
db.myCollection.insert( { y: 2 } );
db.myCollection.find().pretty() # print collection
db.myCollection.find({x:1}).pretty() # find a record matching given query conditions
````





## Graphing Library

If using the [`rails-erd`](https://github.com/voormedia/rails-erd) gem, satisfy [`ruby-graphviz`](https://github.com/glejeune/Ruby-Graphviz) dependency by installing the [`graphviz`](http://graphviz.org/) library.

```` sh
brew install graphviz
````
