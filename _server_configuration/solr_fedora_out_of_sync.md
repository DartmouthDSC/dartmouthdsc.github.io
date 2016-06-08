---
title: Solr and Fedora out of sync
---
If there are items in Solr that no longer have an object in Fedora, the Solr document in Fedora needs to be deleted. To delete the objects in Solr run the following line for each object:
`ActiveFedora::SolrService.instance.conn.delete_by_id("")`

Then in your Solr panel reload the core that the application is using.
