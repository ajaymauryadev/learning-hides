# TaskForge Initial Development Phases

## Current status

Yeh dependency-based sequence plan hai. Future phase ko is document ke exist karne
se implemented nahi maana jayega.

## Path to first database-backed vertical slice

```text
Phase 0  Mental model/product foundation
  -> Phase 1  Terminal, files, environment
  -> Phase 2  Git foundation
  -> Phase 3  Backend JavaScript
  -> Phase 4  Node.js
  -> Phase 5  npm
  -> Phase 6  HTTP/networking
  -> Phase 7  Express
  -> Phase 8  Configuration/startup
  -> Phase 9  MongoDB
  -> Phase 10 Mongoose connection
  -> Phase 11 Domain/data modelling
  -> Phase 12 First schema/model
  -> Phase 13 API architecture layers
  -> Phase 14 First create-API vertical slice
```

## Milestones

| Phase | Why it comes here | Practical milestone |
|---:|---|---|
| 0 | Shared vocabulary and direction | Product, roadmap, architecture and learning state |
| 1 | Developer must operate tools/paths | Verified environment and project root |
| 2 | Changes need safe history | Repository, ignore rules and initial commit |
| 3 | Backend code uses JavaScript | Language exercises understood |
| 4 | JavaScript backend needs runtime | First Node program/process operation |
| 5 | Libraries/scripts need management | Reproducible Node workspace |
| 6 | API relies on HTTP concepts | Native HTTP server and round trip |
| 7 | Routing/middleware simplify HTTP app | Verified Express API/liveness |
| 8 | Credentials/startup need safe config | Fail-fast configuration/startup |
| 9 | Durable data needs DB foundation | Safe MongoDB setup understanding |
| 10 | Node must connect reliably | Connection/readiness lifecycle |
| 11 | Requirements must become data design | Entity relationship design |
| 12 | Data rules need executable interface | First validated User model |
| 13 | Responsibilities need boundaries | Feature structure/vertical-slice design |
| 14 | Parts must work end-to-end | Tested create API from boundary to DB and back |

## Advancement rule

Next topic/phase tabhi start hoga jab current topic ka required artifact aur
proportional verification record ho. File existence alone completion evidence nahi.
Reusable evidence gate `notes/DEFINITION_OF_DONE.md` mein defined hai.
