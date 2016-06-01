---
title: Loaders
---
**Note** This information is specific to the implementation at Dartmouth.
{: .notice--info}

## Introduction
There are series of classes that are responsible for loading employees, organizations and documents into the Linked Name Authority. Each type of resource has its own class that inherits from the main loader class. Each class has a generalized instance method usually called something like `into_lna` that takes a hash with the information about one particular resource. Each class also has class methods that load the data from a particular source, these methods are usually called something like `from_oracle_table`.

The order resource types are loaded into the LNA is really important because a person must have an organization and documents must be related to a person. Therefore, the order of a load should be as follows:

1. Organizations
2. People
3. Documents

After each load, a row is added the Import table that stores information about the load. The most pertinent piece of information stored is when the load started and ended. This isn't important for all the loads but it is crucial if we are only trying to load data that has been updated.

A very brief overview of each class is described below, please visit see the inline documentation [here](https://github.com/DartmouthDSC/LinkedNameAuthority/tree/develop/app/utilities/load) for detailed information.

## Organization Loader
The organization loader, `Load::Organizations.from_hr`, read in date from an Oracle view that contains information about each organization at Dartmouth. The `organization_id` in the table is used to match organization records in the LNA to records in the oracle table. 

## Employee Loader
The employee loader, `Load::People.from_hr`, reads in data from an HRMS Oracle view that contains appointments for faculty and employees. If a person has multiple appointments, they will have multiple rows in the table. Their primary appointment will have a primary flag. Therefore, primary appointments should be loaded first followed by all other appointments. The employee loader takes care of creating the person object, account object and appointment object. If a person with the same netid has already been created, it uses that person object

## Document Loader
The document loader, `Load::Documents.from_elements` reads data from the Symplectic Elements API. It queries for all the users that have been updated since the last load, then imports any publications that have not previously been loaded. Essentially, each document is only loaded once--any changes made in Elements will not be picked up by the LNA.
