---
title: Change Event Logic
---

### A change event should be trigged when:
- an org is created
- an org is destroyed, assuming it turns into a new org
- organizations are merged

### A change event should NOT be trigged when:
- changes are made to the label, alt_label, start_date, po_box, purpose
- memberships are added or removed
- accounts are added or removed
- people are added or removed
- super and sub organizations are added or removed

# Pseudo Code Examples
(In all these examples orgs are active, unless otherwise stated.)

### org A -> historic org A
1. copy over memberships, people, resulted_from, hr_id, alt_label, label, begin_date, kind, hinman_box to historic A
2. add end_date to historic A
3. recursively serialize org:subOrganizationOf relationships for A up to root and hasSubOrganization relationship up to the last child organization.
4. add serialization to historic A
5. add changed_by change event (if needed) to historic A
6. destroy a (potentially need to remove all other relationships before destroying.)

### org A -> org B
1. Create ChangeEvent by:
   - using org B's resulted_from change event
   - creating new change_event based on date and description
2. Create org B
3. Search org:Memberships for org:organization relationships to A
  - Having an end date means that end date is set and is before or on Today's date
  - if they have an end date on their org:memberDuring, leave them alone 
  - if they do not have an end date, change org:organziation relationship from A to B
4. Search for people, who's primary organization is A, for each person
  - if the person has no active memberships, person should stay with historic org
  - if the person has an active membership related to A, then then the person should move to B
  - if the person has an active membership related to C, its primary org should be set to C. (if there are multiple active memberships, just pick the first).
5. migrate accounts from A to B
6. copy org:suborganizationOf and org:hasSubOrganization from A to B.
7. make A a historic org (following instruction above)
8. set ChangeEvent relationships to A and B

### historic org A -> org B
1. Create ChangeEvent by:
   - using org A's changed_by change event
   - using org B's resulted_from change event
   - creating new change_event based on date and description
2. create org B (if not yet created)
3. set ChangeEvent relationships to A and B


### org A -> org B & org C

1. create ChangeEvent
2. create org B & C
3. set primary child org, either B or C.
4. search org:Memberships for org:organization relationships to A
   - if they have an end date on their org:memberDuring, leave them alone
   - if they do not have an end date, change org:organziation relationship from A to primary child org
add lna:wasActive interval to A
recursively serialize org:subOrganizationOf relationships for A up to root
add lna:historicPlacement to A, store serialization in it
migrate org:suborganizationOf and org:hasSubOrganization from A to primary child org. (A should have no relationships left at this point, other than to changeEvents and Memberships that are no longer active)
set ChangeEvent relationships to A, B, and C

### org A & org B -> org C

create ChangeEvent
create org C
search org:Memberships for org:organization relationships to A & B
- if they have an end date on their org:memberDuring, leave them alone
- if they do not have an end date, change org:organziation relationship from A/B to C
add lna:wasActive interval to A & B
recursively serialize org:subOrganizationOf relationships for A & B up to root
add lna:historicPlacement to A & B, store serialization in it
migrate org:suborganizationOf and org:hasSubOrganization from A & B to C. (A & B should have no relationships left at this point, other than to changeEvents and Memberships that are no longer active)
set ChangeEvent relationships to A, B, and C