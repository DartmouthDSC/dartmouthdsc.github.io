---
title: Setting up your development environment
---

1. The Oracle client and Postgres requires setting environment variables in `~/.bash_profile`. Add this to that file before running bundle install:

   ``` bash     
   # Oracle Definitions.
   export ORACLE_BASE=/usr/lib/oracle
   export ORACLE_HOME=$ORACLE_BASE/12.1/client64

   export LD_LIBRARY_PATH=/usr/lib/oracle/12.1/client64/lib:$LD_LIBRARY_PATH
   export PATH=$PATH:/usr/lib/oracle/12.1/client64/bin
    
   # Sets Characters Oracle is using.
   export NLS_LANG=AMERICAN_AMERICA.WE8ISO8859P1
    
   # Adding Postgresql commands to PATH.
   export PATH=$PATH:/usr/pgsql-9.5/bin
   ```

2. Install all gem dependencies:

   ```shell
   bundle install
   ```
   Note: If running in *production* use `bundle install --without development test ci`

3. Once everything is installed, sync the db:

   ```
   rake db:migrate
   ```

4. Set environment variables in `.env` (You may need to create a new file). See [Environment Variables](/lna/techdocs/environment_variables) for a description of variables needed.

5. Load organizations, people (faculty) and documents:

   ```
   rake load:all
   ```
   
6. To write crontab:

   ```
   whenever -w
   ```
   
   **Note:** This will write the crontab under the current user running this command. 

   **Note:** If you would like the commands in the crontab to run in a different environment than `development`, either use the `--set` flag avaliable in `whenever` or set the rails environment before running the command `RAILS_ENV=qa whenever -w`.

7. To seed db (with roles and privilages for developers):

   ```
   rake db:seed
   ```