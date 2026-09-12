# TaskForge Primary Use Cases

## Current status

Yeh product requirements ka conceptual catalog hai. API methods/URLs, schemas,
services, database operations aur tests abhi implement nahi hue.

## Use-case template

- Actor: goal initiate karne wala role.
- Goal: actor kya outcome chahta hai.
- Trigger: interaction kis action/event se start hoti hai.
- Preconditions: start se pehle kya true hona chahiye.
- Main flow: successful business steps.
- Alternative/failure flow: invalid, unauthorized or unavailable conditions.
- Postcondition: success/failure ke baad system state kya honi chahiye.

## Primary catalog

| ID | Actor | Goal | Expected outcome |
|---|---|---|---|
| UC-01 | Visitor/registered user | Account register and authenticate | Safe account/session outcome |
| UC-02 | Registered user | Own profile manage | Allowed profile fields updated |
| UC-03 | Registered user | Workspace create | User becomes initial owner |
| UC-04 | Workspace owner/admin | Member invite/manage | Valid membership lifecycle change |
| UC-05 | Workspace owner/admin | Project create/manage | Workspace-scoped project maintained |
| UC-06 | Authorized member | Task create/read/update | Valid task state maintained |
| UC-07 | Authorized member | Task assign/change status | Allowed assignment/state transition |
| UC-08 | Authorized member | Comment/collaborate | Workspace-scoped discussion recorded |
| UC-09 | Authorized member | Search/filter/list work | Allowed relevant results returned |
| UC-10 | Authorized user | Notifications receive/manage | Relevant delivery/read state maintained |
| UC-11 | Workspace owner | Workspace archive | Controlled lifecycle change |
| UC-12 | System administrator | Health/audit/moderation work | Protected platform operation completed |

## Cross-cutting expectations

- Identity and workspace/resource scope verified when required.
- Invalid/unauthorized attempts do not perform forbidden side effects.
- Safe success/failure outcome returned.
- Sensitive information excluded.
- Meaningful state changes eventually become testable and auditable.

