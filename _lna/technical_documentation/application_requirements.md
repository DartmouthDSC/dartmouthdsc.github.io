---
title: Application Requirements
---

There are many ways to setup your environment, this guide is meant to provide some guidance but it is by no means the only way to setup the environment. For specifics about Dartmouth's installation, please visit the [installation guide](/server_configuration/installation_guide).

## Ruby (+ RVM or another ruby manager)
With or without a ruby manager install Ruby 2.3.x. The LNA might work with 2.2.x versions of Ruby, though we are not testing to make sure this is true.

## Fedora 4.x
This application uses Fedora Commons 4 and will not work with Fedora Commons 3. By default, the application will connect to Fedora on port 8080.

For instructions on setting up Fedora please visit <https://wiki.duraspace.org/display/FEDORA4x/Deploying+Fedora+4+Complete+Guide>

We deploy Fedora using Jetty 9.

A few of the 4.x releases have been used to develop the application and none have presented any problems.

## Solr
This application uses Solr 5.x, we have not tested with previous version of Solr. By default, the application configuration will connect to Solr on port 8983. For more information on the names of the Solr cores needed by the application, please see [config/solr.yml](https://github.com/DartmouthDSC/LinkedNameAuthority/blob/master/config/solr.yml).

We are using the Solr configuration used by active_fedora, it can be found [here](https://github.com/projecthydra/active_fedora/tree/master/solr/config). In the LNA git repository, we are keeping snapshots of the configuration at <https://github.com/DartmouthDSC/LinkedNameAuthority/tree/master/solr/config>.

## Apache + Passenger (or another web server capable of deploying Rails applications)
Install Apache 2.2 and Passenger 5.x.

## Postgresql
We are using Postgresql as our database for our development, test and production environments.
If you do not want to use Postgres, update the database configuration in `config/database.yml` and if Postgres isn't being used at all remove `pg` from the `Gemfile` at the root of the application.
