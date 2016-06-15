---
title: Step-by-Step Guide
---
{% include toc title="Guide" %}

This guide covers running the Linked Name Authority in a development or production environment. In this guide, we try to skip over any requirements that are specific to the Dartmouth implementation of the Linked Name Authority.

## Setting up development environment

### 1. Install Ruby, RVM, and Git
If not already installed, use these [instructions](http://installfest.railsbridge.org/installfest/) to install Ruby, RVM, Rails and Git.

### 2. Clone Git Repository
In a directory of your choosing clone the repository:

```
$ git clone https://github.com/DartmouthDSC/LinkedNameAuthority.git
```

### 3. If necessary, add `.env` file
Initially none of the ENV variables described are necessary, if you are using the loaders, Cron, Elements, Oracle or Postgresql then the corresponding ENV variables may be necessary. To add ENV variables, create a `.env` file at the root of the app directory with any environment variables necessary. More information about LNA specific environment variables can be found [here](environment_variables).

### 4. Change Development Configuration
Because most development environments don't run Postgresql databases, update the contents in `config/database.yml` to reflect the following configuration for development.

``` yaml
development:
  adapter: sqlite3
  pool: 5
  timeout: 5000
  database: db/development.sqlite3
```

Add the sqlite3 gem, by adding the following line to the application's `Gemfile`:

```
gem 'sqlite3'
```

Turn off ssl in `config/environments/development.rb` by setting the following flag to false:

``` ruby
config.force_ssl = false
```

### 5. Install All Gems
Install all dependent gems, if you are using Oracle or Postgresql in your installation do not remove the required gems by using the `-without` flag shown below.

```
$ gem install bundler
$ bundle install --without oracle pg
```

### 6. Run Solr and Fedora
For the purposes of this guide, we are using [fcrepo\_wrapper](https://github.com/cbeer/fcrepo_wrapper) and [solr\_wrapper](https://github.com/cbeer/solr_wrapper) to run Solr and Fedora.

Open a new terminal and cd into the application directory, then run the following command to run Solr:

```
$ solr_wrapper
```

Open another new terminal and cd into the application directory, then run the following command to run Fedora:

```
$ fcrepo_wrapper
```

**Note** Fedora 4 requires Java 8, please check your Java version if you get errors.
{: .notice--warning}

### 7. Run Migrations

```
$ rake db:migrate
```

### 8. Run Rails Server
In another terminal cd into the application directory and run:

```
$ rails server
```

In a web browser go to <http://localhost:3000/> and the application should be running.

**Note** Currently, Dartmouth CAS authentication is required to see the html pages. In order to see these pages, authentication has to be removed or disabled. Instead, you can see the information stored by using the API (we'll cover this a later).
{: .notice--warning}

### 9. Create LNA objects
Open a console to create records for the application, run `rails c`.

Because every person must be related to an organization, create an organization:

``` ruby
org = Lna::Organization.create!(label: "Department of Unicorns and Fairies", begin_date: Date.today)
```

Now, add a person:

``` ruby
person = Lna::Person.create!(full_name: 'Jane Doe', given_name: 'Jane', family_name: 'Doe', primary_org: org)
```

Now, add a membership to the person:

``` ruby

membership = Lna::Membership.create!(title: 'Chair of the Department', begin_date: Date.today, organization: org, person: person)
```

Now, add an account to the person:

``` ruby
account = Lna::Account.create!(title: 'Dartmouth', account_name: 'd0000a', account_service_homepage: 'dartmouth.edu', account_holder: person)

```

Now, add a document(work) to a person's collection:

``` ruby
doc = Lna::Collection::Document.create!(collection: person.collections.first, title: 'Unicorns and Magic', author_list: ['Jane Doe'])
```

### 10. Browse the API
At <http://localhost:3000/persons.jsonld> you should see a record for the person you created. If you append `.jsonld` to the URI listed as the `@id` and visit the webpage you should see the membership and the account you just created for this person. An example is shown below:

``` json
{
  "@context": {
    "foaf": "http://xmlns.com/foaf/0.1/",
    "skos": "http://www.w3.org/2004/02/skos/core#",
    "vcard": "http://www.w3.org/2006/vcard/ns#",
    "owltime": "http://www.w3.org/2006/time#",
    "dc": "http://purl.org/dc/terms/"
  },
  "foaf:primaryTopic": "http://localhost:3000/person/a6604df4-c8bf-4816-bd9b-537031ed5770",
  "generatedAt": "2016-06-01T14:08:24Z",
  "status": "success",
  "@graph": [
    {
      "@id": "http://localhost:3000/person/a6604df4-c8bf-4816-bd9b-537031ed5770",
      "@type": "foaf:Person",
      "foaf:name": "Jane Doe",
      "foaf:givenName": "Jane",
      "foaf:familyName": "Doe",
      "foaf:title": "",
      "foaf:mbox": null,
      "foaf:mbox_sha1sim": "",
      "foaf:image": "",
      "foaf:homepage": [

      ],
      "org:reportsTo": "http://localhost:3000/organization/a25e5d25-c1fe-49aa-8df6-a2d226a7d138",
      "foaf:publications": "http://localhost:3000/person/a6604df4-c8bf-4816-bd9b-537031ed5770/works",
      "@reverse": {
        "org:member": [
          {
            "@id": "#7673d04f-f7ec-44ce-b899-f8242fac762d"
          }
        ]
      },
      "foaf:account": [
        {
          "@id": "#2d232f6d-2b69-46c6-a09a-e58b2ba63019"
        }
      ]
    },
    {
      "@id": "#7673d04f-f7ec-44ce-b899-f8242fac762d",
      "@type": "org:Membership",
      "org:organization": "http://localhost:3000/organization/a25e5d25-c1fe-49aa-8df6-a2d226a7d138",
      "vcard:email": "",
      "vcard:title": "Chair of the Department",
      "vcard:street-address": "",
      "vcard:post-office-box": "",
      "vcard:postal-code": "",
      "vcard:country-name": "",
      "vcard:locality": "",
      "owltime:hasBeginning": "2016-06-01T00:00:00Z",
      "owltime:hasEnd": ""
    },
    {
      "@id": "#2d232f6d-2b69-46c6-a09a-e58b2ba63019",
      "@type": "foaf:OnlineAccount",
      "dc:title": "Dartmouth",
      "foaf:accountName": "d0000a",
      "foaf:accountServiceHomepage": "dartmouth.edu"
    },
    {
      "@id": "http://localhost:3000/organization/a25e5d25-c1fe-49aa-8df6-a2d226a7d138",
      "@type": "org:Organization",
      "skos:prefLabel": "Department of Unicorns and Fairies"
    }
  ]
}
```

At <http://localhost:3000/organizations.jsonld> you see see a record for the organization created. An example is shown below:

``` json
{
  "@context": {
    "org": "http://www.w3.org/ns/org#",
    "owltime": "http://www.w3.org/2006/time#",
    "skos": "http://www.w3.org/2004/02/skos/core#",
    "vcard": "http://www.w3.org/2006/vcard/ns#"
  },
  "@id": "http://localhost:3000/organizations/1",
  "generatedAt": "2016-06-01T14:12:39Z",
  "status": "success",
  "@graph": [
    {
      "@id": "http://localhost:3000/organization/a25e5d25-c1fe-49aa-8df6-a2d226a7d138",
      "@type": "org:Organization",
      "skos:prefLabel": "Department of Unicorns and Fairies",
      "org:identifier": "",
      "skos:altLabel": [

      ],
      "org:purpose": "",
      "vcard:post-office-box": "",
      "owltime:hasBeginning": "2016-06-01T00:00:00Z",
      "owltime:hasEnd": "",
      "org:subOrganizationOf": [

      ]
    }
  ]
}
```


## Setting up production environment

### 1. Install Application Requirements
After all the [application requirements](application_requirements) are met, getting the application started should be similar to starting any rails application. We use Capistrano to make deployment easier, but its definitely not a requirement.

### 2. Clone Git Repository
In a directory of your choosing run:

``` bash
$ git clone git@github.com:DartmouthDSC/LinkedNameAuthority.git
```

### 3. Add `.env` File
Create a `.env` at the root of the app directory with any environment variables necessary. More information about environment variables can be found [here](environment_variables). Pay close attention to variables needed in production.

### 4. Install All Gems

```
$ gem instal bundler
$ bundle install --without oracle development test ci
```

### 5. Run Migrations

```
$ RAILS_ENV=production rake db:migrate
```

### 6. Precompile Assets

```
$ RAILS_ENV=production rake assets:precompile
```
