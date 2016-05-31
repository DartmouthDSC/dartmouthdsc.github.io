---
title: Step-by-Step Guide
---
{% include toc title="Guide" %}

This guide covers getting up the Linked Name Authority in  a development or production environment.

## Setting up development environment

### 1. Install Ruby, RVM, and Git
If not already installed, use these [instructions](http://installfest.railsbridge.org/installfest/) to install Ruby, RVM, Rails and Git.

### 2. Clone Git Repository
In a directory of your choosing run:

```
$ git clone git@github.com:DartmouthDSC/LinkedNameAuthority.git
```

### 3. If necessary, add `.env` file
Initially none of the ENV variables described are necessary, if you are using the loaders, Elements, Oracle or Postgresql then the corresponding ENV variables may be necessary. To add ENV variables, create a `.env` at the root of the app directory with any environment variables necessary. More information about environment variables can be found [here](environment_variables). 

### 4. Change Development DB Configuration
Because most development environments don't run Postgresql databases update the contents in `config/database.yml` to reflect the following configuration for development.

``` yaml
development:
  adapter: sqlite3
  pool: 5
  timeout: 5000
  database: db/development.sqlite3
```

And add the sqlite3 gem. In the application's `Gemfile` add the following line:

```
gem 'sqlite3'
```

### 5. Install All Gems
Install all dependent gems, if you are using Oracle in your installation do not add the `-without` flag shown below.

```
$ gem install bundler
$ bundle install --without oracle pg
```

### 6. Run Solr and Fedora
For the purposes of this guide, we are using [fcrepo\_wrapper](https://github.com/cbeer/fcrepo_wrapper) and [solr\_wrapper](https://github.com/cbeer/solr_wrapper) to run Solr and Fedora.

Open a new terminal and cd into the application directory, then run the following command to run Solr:

```
solr_wrapper
```

Open another new terminal and cd into the application directory, then run the following command to run Fedora:

```
fcrepo_wrapper
```

### 7. Run Migrations

```
$ rake db:migrate
```

### 8. Run Rails Server
In another terminal cd into the application directory and run:

```
rails server
```

In a web browser go to `localhost:3000/` and the application should be running.

## Setting up production environment

### 1. Install Application Requirements
After all the [application requirements](application_requirements) are met, getting the application started should be similar to starting any rails application. We use Capistrano to make deployment easier, but its definitely not a requirement.

### 2. Clone Git Repository
In a directory of your choosing run:

``` bash
$ git clone git@github.com:DartmouthDSC/LinkedNameAuthority.git
```

### 3. Add `.env` File
Create a `.env` at the root of the app directory with any environment variables necessary. More information about environment variables can be found [here](environment_variables). Pay close attention to variables needed in production.

### 4. Install All Gems

```
$ gem instal bundler
$ bundle install --without oracle development test ci
```

### 5. Run Migrations

```
$ RAILS_ENV=production rake db:migrate
```

### 6. Precompile Assets 

```
$ RAILS_ENV=production rake assets:precompile
```
