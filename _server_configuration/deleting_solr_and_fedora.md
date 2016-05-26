---
title: Deleting Solr and Fedora
---
**ONLY FOR DEVELOPMENT**

1. Open a console for the correct environment: `rails c development`
2. In the console run:

   ```
   require 'active_fedora/cleaner'
   ActiveFedora::Cleaner.clean!
   ```
