# Topic 12 — TaskForge ke users

## Aaj ka exact objective

Aaj identify karna hai ki TaskForge ko kaun use/operate karega aur authority ka scope
kya hoga. Detailed workflows Topic 13 mein aur actual authorization Phase 21 mein
aayegi. Aaj schema, API ya role code create nahi hoga.

## User aur role same nahi

**User/account** system mein person ki identity represent karta hai. **Role** kisi
specific context mein us user ki authority/responsibility describe karta hai.

```text
Ajay = one registered user account
Ajay in Workspace A = owner
Ajay in Workspace B = member
```

Isliye `owner` permanent global user type nahi; workspace-scoped role hai.

## 1. Registered or normal user

Registered user ka TaskForge account hota hai. Planned capabilities:

- register/login/logout;
- own profile manage;
- workspace create;
- invitation ke through workspace join;
- allowed workspaces/projects/tasks par work.

Workspace create karne par user us specific workspace ka owner ban sakta hai.

## 2. Workspace owner

Workspace owner specific workspace ki highest business authority hai. Planned scope:

- workspace manage/archive;
- members invite/remove;
- workspace roles assign/change;
- important settings manage;
- ownership safety rules follow.

Owner platform ka system administrator automatically nahi hota.

## 3. Workspace admin

Workspace admin ko owner delegated management authority deta hai. Planned scope:

- projects manage;
- members ke allowed management operations;
- limited workspace settings;
- normal collaboration work.

Admin ki authority owner se limited ho sakti hai. Example: last owner ko remove ya
workspace permanently control karna allowed na ho.

## 4. Workspace member

Workspace member normal collaborative role hai. Planned scope:

- accessible projects/tasks dekhna;
- assigned/allowed task update;
- comments add/edit within rules;
- permission ke andar collaboration.

Member ko owner/admin operations automatically allowed nahi.

## 5. System administrator

System administrator platform-level operational role hai. Planned responsibilities:

- system health/operational metrics;
- protected audit-log access;
- platform-level moderation;
- incidents/abuse handle karna.

System admin aur workspace admin different scopes hain:

```text
Workspace admin = one workspace ka delegated manager
System admin    = TaskForge platform operations/moderation
```

System admin ko bhi unlimited casual data access nahi milna chahiye. Least privilege,
auditing aur sensitive-data protection required honge.

## Role matrix — conceptual

| Capability | Normal user without membership | Member | Workspace admin | Workspace owner | System admin |
|---|---:|---:|---:|---:|---:|
| Own profile manage | Yes | Yes | Yes | Yes | Yes |
| Accessible tasks collaborate | No | Yes | Yes | Yes | Platform role se automatically nahi |
| Project/member management | No | Limited/no | Planned allowed subset | Yes | Platform role se automatically nahi |
| Workspace archive/control | No | No | Limited/no | Yes | Moderation rule par depend |
| Platform health/moderation | No | No | No | No | Yes |

Matrix final API contract nahi. Exact permissions later requirements aur tests ke
saath define honge.

## Authentication vs authorization recap

- **Authentication:** “Aap kaun hain?”
- **Authorization:** “Is context mein aapko yeh operation allowed hai?”

Login ho jana har workspace ke data ka access nahi deta.

```text
Authenticated user
  -> workspace membership check
  -> role/permission check
  -> resource/ownership rule
  -> allow or deny
```

## Multi-workspace example

```text
User: Riya
Workspace Alpha: owner
Workspace Beta: admin
Workspace Gamma: member
Workspace Delta: no membership
```

Riya ki authority current workspace ke relationship se decide hogi. Alpha ka owner
hona Delta ka data access nahi deta. Yeh tenant/workspace isolation ka foundation hai.

## Ownership, role aur permission

- **Ownership:** resource/control kis person/account se associated hai.
- **Role:** context mein authority grouping.
- **Permission:** specific operation karne ki allowed capability.

Example:

```text
Role: workspace admin
Permission: project create karna
Ownership rule: workspace owner ko admin remove nahi kar sakta
```

Exact data modelling future phases mein hoga.

## Least privilege

User ko utni hi authority deni chahiye jitni responsibility ke liye required hai.
Member ko owner permissions dena convenient lag sakta hai, lekin accidental/malicious
damage ka risk badhata hai.

## Default deny

Sensitive operation ke liye clear permission evidence na mile to safest default deny
hai.

```text
No verified membership/permission -> operation not allowed
```

## UI button permission nahi

Frontend member ko “Archive Workspace” button hide kar sakta hai, lekin modified
client direct operation attempt kar sakta hai. Backend ko role, permission aur
resource scope independently verify karna hoga.

## User lifecycle ka preview

Future system ko handle karna hoga:

- registered but no workspace;
- invited but not accepted;
- active member;
- role changed;
- member removed;
- user deactivated;
- workspace archived.

Lifecycle implementation relevant later topics mein aayegi.

## Common misconceptions

1. **"Normal user aur member same hain."**  
   Account platform-level identity hai; member specific workspace relationship hai.

2. **"Owner global role hai."**  
   Owner specific workspace context mein hota hai.

3. **"Workspace admin system admin hai."**  
   Dono ke authority scopes different hain.

4. **"Login ke baad sab workspaces accessible hain."**  
   Authentication alone authorization nahi.

5. **"System admin ko har private record casually dekhna chahiye."**  
   Platform authority bhi least privilege, purpose and audit controls follow kare.

6. **"Frontend button hidden hai, permission protected hai."**  
   Backend enforcement ke bina secure nahi.

## Verification strategy

Topic understood hai agar learner:

1. user/account aur role distinguish kare;
2. four workspace/platform roles explain kare;
3. workspace admin aur system admin separate kare;
4. same user ke different workspace roles ka example de;
5. authentication aur authorization distinguish kare;
6. least privilege/default deny ka basic reason bataye.

## Quick self-check

1. Registered user aur workspace member mein kya difference hai?
2. Owner ki authority global hai ya workspace-scoped?
3. Workspace admin aur system admin mein difference?
4. User owner ek workspace aur member doosre mein ho sakta hai?
5. Login hona permission kyun prove nahi karta?
6. Hidden UI button security control kyun enough nahi?

## Practice exercise

Scenario:

```text
Neha Workspace A ki owner, Workspace B ki member aur Workspace C mein non-member hai.
```

Answer karo:

```text
Workspace A mein role:
Workspace B mein role:
Workspace C mein relationship:
Kaunsi check identity establish karegi:
Kaunsi check operation authority establish karegi:
Member ko owner operation deny karne ka reason:
```

<details>
<summary>Answer-after-attempt</summary>

```text
Workspace A: owner
Workspace B: member
Workspace C: no membership
Identity: authentication
Authority: authorization using membership/role/permission/resource rules
Deny reason: least privilege and workspace-scoped authority
```

</details>

## Interview question with Hinglish answer

**Question:** TaskForge mein users aur roles kaise organize honge?

**Answer:** TaskForge mein registered account platform identity represent karega,
aur owner, admin ya member role workspace membership ke context mein hoga. Same user
different workspaces mein different roles rakh sakta hai. Workspace owner highest
workspace authority, admin delegated manager aur member collaborator hoga. System
administrator separate platform-level operational role hoga. Backend har operation
par authentication ke baad workspace-scoped authorization enforce karega.

## Easy-English minimum interview answer

**TaskForge has registered users and workspace-scoped roles: owner, admin, and
member. The same user can have different roles in different workspaces. A system
administrator is a separate platform-level role. The backend must check both the
user's identity and their permission for the current workspace and resource.**

### Even shorter version

**TaskForge uses workspace-scoped owner, admin, and member roles, plus a separate
platform-level system administrator role.**

## Topic boundary

Topic 12 mein TaskForge users aur role scopes complete hue. **Primary use cases**
Topic 13 ko iske baad separately complete kiya gaya.
