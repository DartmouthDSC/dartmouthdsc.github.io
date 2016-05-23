---
title: Environment Variables
---

Listed below are environment variables needed by the Linked Name Authority application.

These variables should be added to `.env` at the root of the app directory. [dotenv](https://github.com/bkeepers/dotenv) loads the environment variables.

``` bash
# Email list used by loaders to send email notifications about warnings and errors.
# Optional
LOADER_NOTICES=me@example.com

# Email list used by loaders to send email notifications about errors.
# Required
LOADER_ERROR_NOTICES=me@example.com,me.too@examples.com

# Email to send cron errors
# Required
# CRON_EMAIL_NOTICES=me@example.com

# Credentials for Elements
# Required
ELEMENTS_USERNAME=******
ELEMENTS_PASSWORD=*********

# Credentials for Oracle.
# Required
LNA_ORACLE_USERNAME=****
LNA_ORACLE_PASSWORD=********

# Credentials for Postgresql
# Required
POSTGRES_USER=*****
POSTGRES_PASSWORD=*************
```

For qa and production environments, these additional variables are needed.

``` bash
# Used to sign cookies. Create a new key by running `rake secret`.
# Required
SECRET_KEY_BASE=******************
```