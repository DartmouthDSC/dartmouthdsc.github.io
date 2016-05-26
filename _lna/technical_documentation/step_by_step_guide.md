---
title: Step-by-Step Guide
---

## Running the application in development with fcrepo_wrapper and solr_wrapper



## Running the application in production

After all the [application requirements](application_requirements) are met, getting the application started should be similar to starting any rails application.

### Clone git repository
In a directory of your choosing run:

``` shell
git clone git@github.com:DartmouthDSC/LinkedNameAuthority.git
```

### Add `.env` file
Create a `.env` at the root of the app directory with any environment variables necessary. More information about environment variables can be found [here](environment_variables).


### Install Bundler

``` shell
gem bundler install
```

### Bundler Install
Install all dependent gems, if you are using Oracle in your installation do not add the `-without` flag shown below.

```
bundle install --without oracle
```





