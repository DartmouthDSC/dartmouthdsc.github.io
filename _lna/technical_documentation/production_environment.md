---
title: Production Environment
---

# Running an application in production without Capistrano

  ```
  bundle install
  
  rake assets:precompile RAILS_ENV=production
	
  rake db:migrate RAILS_ENV=production
  ```
