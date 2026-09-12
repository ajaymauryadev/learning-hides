# TaskForge Users and Role Scopes

## Current status

Yeh conceptual domain definition hai. User account, membership schema, authentication
aur authorization abhi implement nahi hue.

## Actor categories

| Actor/role | Scope | Planned responsibility |
|---|---|---|
| Registered/normal user | Platform account | Register/login, profile manage, workspace create/join |
| Workspace owner | One specific workspace | Workspace control, members/roles, archive and ownership safeguards |
| Workspace admin | One specific workspace | Delegated project/member/settings management |
| Workspace member | One specific workspace | Allowed projects/tasks/comments par collaboration |
| System administrator | Whole TaskForge platform | Health, operational metrics, protected audit and moderation |

## Critical rule

Workspace role membership relationship par depend karega, user account par globally
nahi. Same account different workspaces mein different roles rakh sakta hai.

## Security principles

- Authentication identity establish karegi.
- Authorization requested operation ko current scope mein allow/deny karegi.
- Least privilege: required minimum authority.
- Default deny: explicit permission evidence na ho to sensitive operation allow nahi.
- UI visibility final security control nahi; backend independently enforce karega.

