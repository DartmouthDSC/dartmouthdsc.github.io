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

## Troubleshooting

### Deleting base path tombstone
If after running the command above you get an error resembling `Ldp::Gone ():`, go to the Fedora admin panel and check that the base path for the application has been created. If it has not been created, there is the chance that the tombstone of the base path was not deleted correctly. From the machine where fedora is running, use the instructions [here](https://wiki.duraspace.org/display/FEDORA40/RESTful+HTTP+API+-+Containers#RESTfulHTTPAPI-Containers-RedDELETEDeletearesource) to delete the tombstone resource.
