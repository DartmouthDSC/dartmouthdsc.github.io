---
title: Linked Name Authority Documentation
---

## About

The Linked Name Authority store information and citations about Dartmouth faculty and staff.

## Source Code
[https://github.com/DartmouthDSC/LinkedNameAuthority](https://github.com/DartmouthDSC/LinkedNameAuthority)

## Getting Started?
Follow our [getting started guide](techdocs/getting_started)

## Technical Documentation

- [API via Apiary](http://docs.linkednameauthority.apiary.io/)
{% assign techdocs = site.lna | where: "kind","techdoc" %}
{% for page in techdocs %}
- [{{ page.title }}]({{ page.url }})
{% endfor %}

## User Documentation

{% assign userdocs = site.lna | where: "kind","userdoc" %}
{% for page in userdocs %}
- [{{ page.title }}]({{ page.url }})
{% endfor %}