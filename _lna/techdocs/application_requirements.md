---
title: Application Requirements
kind: techdoc
---

This is a generalized guide on how to setup the Linked Name Authority. There are many ways to setup your environment, this guide is meant to provide some guidence but it is by no means the only way to setup the environment. For specifics about Dartmouth's installation, please visit the [installation guide](/lna/techdocs/installation_guide).

# Application Requirements

### Ruby (+ RVM or another ruby version manager)
Install Ruby 2.3.x
RubyGems?

### Fedora
This application strictly uses Fedora Commons 4 and will not work with Fedora Commons 3. 
We deploy Fedora using Jetty 9.
We have used a few of the 4.x releases and haven't experienced any problems.

The application configuration is expecting Fedora 4 at port 8080.

### Solr
We have only tested this application using Solr 5.
The application configuration is expecting Solr 5 at port 8983.
will need a solr core named 'lna_dev' if running in development.

### Apache + Passenger (or another web server capable of deploying Rails applications)
Install Apache 2.2 and Passenger 5.x.

### Postgresql
We are using Postgresql as our database for our development, test and production environments. 
If you do not want to use postgres, update the database configuration in `config/database.yml` and if postgres isn't being used at all remove `pg` from the `Gemfile` at the root of the application.





