---
title: Application Requirements
---

This is a generalized guide on how to setup the Linked Name Authority. There are many ways to setup your environment, this guide is meant to provide some guidence but it is by no means the only way to setup the environment. For specifics about Dartmouth's installation, please visit the [installation guide](/lna/techdocs/installation_guide).

# Application Requirements

### Ruby (+ RVM or another ruby version manager)
Install Ruby 2.3.x
RubyGems?

### Fedora
This application uses Fedora Commons 4 and will not work with Fedora Commons 3. By default, the application will connect to Fedora on port 8080.
For instructions on setting up Fedora please visit...

We deploy Fedora using Jetty 9.

A few of the 4.x releases have been used to develop the application.

### Solr
This application uses Solr 5.x, we have not tested uses previous version of Solr. By default, the application configuration will connect to Solr on port 8983. For more information on the solr cores needed by the application, please see `config/solr.yml`.
Using the default hydra configuration for solr, 

### Apache + Passenger (or another web server capable of deploying Rails applications)
Install Apache 2.2 and Passenger 5.x.

### Postgresql
We are using Postgresql as our database for our development, test and production environments. 
If you do not want to use postgres, update the database configuration in `config/database.yml` and if postgres isn't being used at all remove `pg` from the `Gemfile` at the root of the application.





