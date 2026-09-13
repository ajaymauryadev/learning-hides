# TaskForge Backend Roadmap

## Roadmap authority

Complete 46-phase, 1,066-topic ordered curriculum project root ke
`TASKFORGE_BACKEND_MASTER_PLAN.md` mein verbatim preserved hai. Yeh file current
progress ko track karti hai. Topics ko reorder ya skip nahi karna hai.

## Status meanings

- `planned`: topic abhi start nahi hua.
- `learning`: concept samjhaya ja raha hai.
- `implemented`: required practical artifact ban gaya hai.
- `locally verified`: local structural/manual verification pass hui hai.
- `tested`: relevant automated test pass hua hai.
- `revised`: revision exercise complete hui hai.
- `complete`: topic ke current-scope learning, artifact aur verification complete hain.

> Har topic ko har intermediate status se guzarna zaroori nahi. Jaise conceptual
> topic mein code implementation ya automated test applicable nahi ho sakta.

## Phase 0 — Software aur backend ka mental model

| No. | Topic | Prerequisite | Practical outcome | Status |
|---:|---|---|---|---|
| 1 | Software application kya hoti hai? | Basic computer use | Software application ka mental model aur TaskForge product definition ka initial scope | complete |
| 2 | Frontend kya hota hai? | Topic 1 | User-facing layer ka mental model | complete |
| 3 | Backend kya hota hai? | Topics 1–2 | Server-side responsibilities ka mental model | complete |
| 4 | Client aur server kya hain? | Topics 1–3 | Client/server interaction ka mental model | complete |
| 5 | Database kya hai? | Topics 1–4 | Persistent data ka mental model | complete |
| 6 | API kya hai? | Topics 1–5 | Software communication contract ka mental model | complete |
| 7 | Request aur response kya hain? | Topic 6 | Basic communication flow | complete |
| 8 | Runtime kya hota hai? | Topics 1–7 | Code execution environment ka mental model | complete |
| 9 | Source code aur running process ka difference | Topic 8 | File aur active program ka difference | complete |
| 10 | Local development aur production ka difference | Topics 8–9 | Environments ka basic distinction | complete |
| 11 | TaskForge product overview | Topics 1–10 | Product scope ka detailed overview | complete |
| 12 | TaskForge ke users | Topic 11 | User types ka definition | complete |
| 13 | Primary use cases | Topics 11–12 | Main user goals/actions | complete |
| 14 | High-level system diagram | Topics 2–13 | Initial system relationships | complete |
| 15 | Initial development phases | Topics 1–14 | Implementation sequence ka overview | complete |
| 16 | Definition of Done | Topic 15 | Completion criteria | complete |
| 17 | Learning documentation system | Topics 1–16 | Durable learning workflow | complete |

### Phase 0 practical output

- [x] TaskForge product definition ka initial foundation
- [x] Backend roadmap/progress tracker initialized
- [x] Honest current architecture document initialized
- [x] Learning-state document initialized
- [x] Topic 17 individually taught and verified

**Phase 0 status: complete (17/17 topics locally verified).**

Phases 0–1 complete hain. Current next ordered topic Phase 2, Topic 35 — Git kya hai?
It has not started.

## Phase 1 — Terminal, files aur development environment

| No. | Topic | Prerequisite | Practical outcome | Status |
|---:|---|---|---|---|
| 18 | Terminal kya hai? | Phase 0 | Terminal/shell/session/input-output mental model and current terminal evidence | complete |
| 19 | PowerShell command anatomy | Topic 18 | Command, arguments, parameters and execution structure | complete |
| 20 | Current working directory | Topic 19 | Current location inspect/understand karna | complete |
| 21 | Absolute aur relative paths | Topic 20 | Filesystem locations safely address karna | complete |
| 22 | File aur folder operations | Topic 21 | Basic safe filesystem operations | complete |
| 23 | VS Code workspace | Topics 20–22 | Project-focused editor workspace mental model | complete |
| 24 | Source file aur configuration file | Topic 23 | Code and configuration responsibilities distinguish karna | complete |
| 25 | File extension ka meaning | Topic 24 | File type/convention interpretation | complete |
| 26 | Hidden files | Topic 25 | Hidden metadata/config files understand karna | complete |
| 27 | Environment verification | Topics 18–26 | Required tools systematically verify karna | complete |
| 28 | Node version | Topic 27 | Installed Node version evidence | complete |
| 29 | npm version | Topic 28 | Installed npm version evidence | complete |
| 30 | Git version | Topic 29 | Installed Git version evidence | complete |
| 31 | VS Code version | Topic 30 | Installed VS Code version evidence | complete |
| 32 | Port aur process ka basic introduction | Topics 18–31 | Basic process/port operating model | complete |
| 33 | Project root folder create karna | Topics 18–32 | `taskforge-backend/` project root | complete |

### Phase 1 practical output

- [x] Current terminal environment identified
- [x] Required development tools verified topic-by-topic
- [x] `taskforge-backend/` project root created at Topic 33
- [x] Topics 19–33 individually taught and verified

**Phase 1 status: complete (16/16 topics locally verified).**

## Phase 2 — Git aur repository foundation

| No. | Topic | Prerequisite | Practical outcome | Status |
|---:|---|---|---|---|
| 34 | Version control kya hai? | Phase 1 | Change-history, snapshot, collaboration and recovery mental model | complete |
| 35 | Git kya hai? | Topic 34 | Git ko distributed VCS ke roop mein samajhna | complete |
| 36 | Repository kya hai? | Topic 35 | Repository boundary and metadata mental model | complete |
| 37 | Working tree | Topic 36 | Current filesystem state ka repository relation | complete |
| 38 | Untracked file | Topic 37 | New unrecorded file state | complete |
| 39 | Tracked file | Topic 38 | Version-controlled file state | complete |
| 40 | Staging area | Topic 39 | Next record selection model | complete |
| 41 | Commit | Topic 40 | Meaningful history-record model | complete |
| 42 | Branch | Topic 41 | Parallel line of development | complete |
| 43 | Remote repository | Topic 42 | Local/shared repository relationship | complete |
| 44 | `git init` | Topics 34–43 | Intentional repository initialization | complete |
| 45 | `git status` | Topic 44 | Repository state inspection | complete |
| 46 | `.gitignore` | Topic 45 | Unwanted/unsecret files exclusion rules | complete |
| 47 | `git add` | Topic 46 | Intended changes stage karna | complete |
| 48 | `git diff` | Topic 47 | Unstaged differences inspect karna | complete |
| 49 | `git diff --cached` | Topic 48 | Staged snapshot inspect karna | complete |
| 50 | `git commit` | Topic 49 | Reviewed local history record create karna | complete |
| 51 | `git log` | Topic 50 | History inspect karna | complete |
| 52 | `git remote` | Topic 51 | Remote mappings inspect/configure karna | complete |
| 53 | `git push` | Topic 52 | Local records remote par publish karna | complete |
| 54 | `git pull` | Topic 53 | Remote changes safely integrate karna | complete |
| 55 | Safe Git workflow | Topics 44–54 | Repeatable inspect-stage-review-record-sync flow | complete |
| 56 | Secrets ko Git se bachana | Topic 55 | Secret prevention and incident response | complete |
| 57 | Initial repository commit | Topic 56 | Reviewed initial TaskForge repository state | complete |

### Phase 2 practical output

- [x] Intentional Git repository boundary: parent repository, no accidental nested `.git`
- [x] Safe `.gitignore`
- [x] TaskForge README
- [x] Reviewed TaskForge foundation completion commit; historical root identified separately
- [x] GitHub remote configured and live verified
- [x] Topics 34–57 individually taught and verified

**Phase 2 status: complete (24/24 topics verified).**

Next ordered topic: **Phase 3, Topic 58 — Statement aur expression.** It has not started.
