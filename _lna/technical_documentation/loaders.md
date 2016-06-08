---
title: Loaders
---
**Note** This information is specific to the implementation at Dartmouth.
{: .notice--info}

## Introduction
Loaders are classes responsible for loading employees, organizations and documents into the Linked Name Authority. Each type of resource has its own class that inherits from the main loader class. Each class has a generalized instance method usually called something like `into_lna` that takes a hash with the information about one particular resource. Each class also has class methods that load the data from a particular source, these methods are usually called something like `from_oracle_table`.

The order resource types are loaded into the LNA is really important because a person must have an organization and documents must be related to a person. Therefore, the order of a load should be as follows:

1. Organizations
2. People
3. Documents

After each load, a row is added the Import table that stores information about the load. The most pertinent piece of information stored is when the load started and ended. This isn't important for all the loads, but it is crucial if we are only trying to load data that has been updated.

A very brief overview of each class is described below, please visit the inline documentation [here](https://github.com/DartmouthDSC/LinkedNameAuthority/tree/develop/app/utilities/load) for detailed information.

## Organization Loader
The organization loader, `Load::Organizations.from_hr`, reads in organization data from an Oracle view provided by Human Resources. That table contains information about each organization at Dartmouth. The table contains both past and present organizations. The `organization_id` column in the table is used to match organizations to records in the LNA.

## Employee Loader
The employee loader, `Load::People.from_hr`, reads in data from an HRMS Oracle view that contains appointments for faculty and employees. If a person has multiple appointments, they will have multiple rows in the table. Their primary appointment will have a primary flag. Primary appointments should be loaded first followed by all other appointments. The employee loader takes care of creating the person object, account object and appointment object. If a person with the same netid has already been created, it uses the same person object.

## Document Loader
The document loader, `Load::Documents.from_elements` reads data from the Symplectic Elements API. It queries for all the users that have been updated since the last load, then imports any publications that have not previously been loaded. Essentially, each document is only loaded once--any changes made in Elements will not be picked up by the LNA.

## Using the Loaders
The LNA's load task to load people, organizations and documents is [here](https://github.com/DartmouthDSC/LinkedNameAuthority/blob/develop/lib/tasks/load.rake).

If there are errors running a load, or if the load is in a weird state re-running the load might  work. But more often than not you will probably want to delete the Import record created by the previously failed load.

If you want a clean slate, you will also need to [delete fedora and solr](/server_configuration/deleting_solr_and_fedora). To delete the entire sql database, run `rake db:reset`.
