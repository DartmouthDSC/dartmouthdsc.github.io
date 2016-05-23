---
title: Deploying to QA
---

## Overview
Capistrano 3.x is used to deploy the application. The application is deployed at `/opt/deploy/LinkedNameAuthority` and is served to the public at at qa.dac.dartmouth.edu/lna. On QA, Capistrano deploys the `develop` branch. If the application has been deployed, before you can skip to step 2.

## Instructions
1. If this is the first time deploying to qa, **on qa.dac**
   - Create empty folders at:

      ```
      /opt/deploy/LinkedNameAuthority/shared
      /opt/deploy/LinkedNameAuthority/shared/db
      ```
   - Create a file called `.env` in `/opt/deploy/LinkedNameAuthority/shared` with necessary environment variables. See 'Environment Variables' section below for a list of required variables.

2. From your **development machine** run:

   ```
   bundle exec cap qa deploy
   ```

   This will checkout the `develop` branch of the application, run any migrations, compile assets, write the crontab and restart apache. 

3. If this is the first time deploying to qa, and you would like to load all the data without waiting for the cron job run:

   ```
   bundle exec cap qa deploy:load_data
   ```

4. If this is the first time deploying to qa, you will probably want to seed the db, with basic roles and privilages for the developers.

   ```
   bundle exec cap qa deploy:seed_db
   ```
