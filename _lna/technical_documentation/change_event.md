---
title: Change Event
---
## Introduction

Major changes in organizations are being tracked by an object called, change event. The object expects to have at least one organization as its original organization and at least one organization as its resulting organization. The change event object contains a description and date of the event. Original organization can only be historic organization (organizations that aren't active). Resulting organization can only be active or historic organizations.

### A change event should be trigged when:

- an org is created
- an org is destroyed and is converted to a new org
- organizations are merged

### A change event should NOT be trigged when:

- changes are made to the label, alt_label, start_date, po_box, purpose
- memberships are added or removed
- accounts are added or removed
- people are added or removed
- super and sub organizations are added or removed

## How To Make Changes in Organizations

A change event can be trigged by using `Lna::Organization::ChangeEvent.trigger_event`.

An organization can be made historic by using `Lna::Organization.convert_to_historic`.

## Pseudo Code Examples
**Note** In all these examples, orgs are active unless otherwise stated.
{: .notice--warning}

### convert org A to historic org A
1. copy over memberships, people, resulted_from, hr_id, alt_label, label, begin_date, kind, hinman_box to historic A
2. add end_date to historic A
3. recursively serialize org:subOrganizationOf relationships for A up to root and hasSubOrganization relationship up to the last child organization.
4. add serialization to historic A
5. add changed_by change event (if needed) to historic A
6. destroy a (before destroying, remove all other relationships and save)

**Note** The id of the historic organization should be the same as the active organization.
{: .notice--danger}

### org A changes to org B
1. Create ChangeEvent by:
   - using org B's resulted_from change event, or
   - creating new change_event based on the date and description given
2. Create org B
3. Search org:Memberships for org:organization relationships to A
  - if they have an end date that is before or on Today's date, leave them alone
  - otherwise, change org:organization relationship from A to B
4. Search for people who's primary organization is A, for each person
  - if the person has no active memberships, person should stay with historic org
  - if the person has an active membership related to A, then then the person should move to B
  - if the person has an active membership related to C, its primary org should be set to C. (if there are multiple active memberships, just pick the first).
5. Migrate accounts from A to B
6. Copy sub and super organizations from A to B.
7. Make A a historic org (following instruction above)
8. Set ChangeEvent relationships to A and B

### historic org A changes to org B
__(Like above but original organization is already historic)__

1. Create ChangeEvent by:
   - using org A's changed_by change event
   - using org B's resulted_from change event
   - creating new change_event based on date and description
2. create org B (if not yet created)
3. set ChangeEvent relationships to A and B


### org A splits into org B & org C
1. create ChangeEvent
2. create org B & C
3. set primary child org, either B or C.
4. search for memberships still related to A
   - if they have an end_date that is before or on Today's date, leave them alone
   - if they do not have an end date, change organization relationship from A to B
5. add prov:atTime interval to A
6. recursively serialize org:subOrganizationOf relationships for A up to root
7. add lna:historicPlacement to A, store serialization in it
8. migrate org:suborganizationOf and org:hasSubOrganization from A to primary child org. (A should have no relationships left at this point, other than to changeEvents and Memberships that are no longer active)
10. set ChangeEvent relationships to A, B, and C

### org A & org B combine to become org C
1. create ChangeEvent
2. create org C
3. For each memberships related to A or B:
  - if they have an end_date before or on Today's date, leave them alone
  - if they do not have an end date, change the organization relationship from A/B to C
4. add prov:atTime interval to A & B
5. recursively serialize org:subOrganizationOf relationships for A & B up to root
6. add lna:historicPlacement to A & B, store serialization in it
7. migrate sub and super organizations from A & B to C. (A & B should have no relationships left at this point, other than to changeEvents and Memberships that are no longer active)
8. set ChangeEvent relationships to A, B, and C
