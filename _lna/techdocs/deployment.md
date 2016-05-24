---
title: QA/Production Deployment
---
## Overview
Capistrano 3.x is used to deploy the application. 

The application is deployed at `/opt/deploy/LinkedNameAuthority`. On **qa.dac.dartmouth.edu**, Capistrano deploys the `develop` branch and the application is served to the public at at qa.dac.dartmouth.edu/lna. On **lna.dartmouth.edu**, Capistrano deploys the `master` branch and is served to the public at lna.dartmouth.edu. If the application has been deployed, before you can skip to step 2.

## Instructions
The instructions for deploying to qa or production are identical. 

1. If this is the first time deploying to qa/production, **on qa.dac/lna**
   - Create an empty folder at:

      ```
      /opt/deploy/LinkedNameAuthority/shared
      ```
   - Create a file called `.env` in `/opt/deploy/LinkedNameAuthority/shared` with necessary environment variables. See [Environment Variables](/lna/techdocs/environment_variables) for a list of required variables.

2. From your **development machine** run:

   ```
   bundle exec cap [qa|production] deploy
   ```

   This will checkout the `develop` or `master` branch of the application, run any migrations, compile assets, write the crontab and restart apache. 

3. If this is the first time deploying to qa/production, and you would like to load all the data without waiting for the cron job run:

   ```
   bundle exec cap [qa|production] deploy:load_data
   ```

4. If this is the first time deploying to qa/production, you will probably want to seed the db, with basic roles and privilages for the developers.

   ```
   bundle exec cap [qa|production] deploy:seed_db
   ```
