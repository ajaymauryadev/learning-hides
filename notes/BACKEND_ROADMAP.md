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

Phase 0 ke baad ordered continuation Phase 1 mein hai. Current next topic:
Topic 19 — PowerShell command anatomy. It has not started.

## Phase 1 — Terminal, files aur development environment

| No. | Topic | Prerequisite | Practical outcome | Status |
|---:|---|---|---|---|
| 18 | Terminal kya hai? | Phase 0 | Terminal/shell/session/input-output mental model and current terminal evidence | complete |
| 19 | PowerShell command anatomy | Topic 18 | Command, arguments, parameters and execution structure | planned |
| 20 | Current working directory | Topic 19 | Current location inspect/understand karna | planned |
| 21 | Absolute aur relative paths | Topic 20 | Filesystem locations safely address karna | planned |
| 22 | File aur folder operations | Topic 21 | Basic safe filesystem operations | planned |
| 23 | VS Code workspace | Topics 20–22 | Project-focused editor workspace mental model | planned |
| 24 | Source file aur configuration file | Topic 23 | Code and configuration responsibilities distinguish karna | planned |
| 25 | File extension ka meaning | Topic 24 | File type/convention interpretation | planned |
| 26 | Hidden files | Topic 25 | Hidden metadata/config files understand karna | planned |
| 27 | Environment verification | Topics 18–26 | Required tools systematically verify karna | planned |
| 28 | Node version | Topic 27 | Installed Node version evidence | planned |
| 29 | npm version | Topic 28 | Installed npm version evidence | planned |
| 30 | Git version | Topic 29 | Installed Git version evidence | planned |
| 31 | VS Code version | Topic 30 | Installed VS Code version evidence | planned |
| 32 | Port aur process ka basic introduction | Topics 18–31 | Basic process/port operating model | planned |
| 33 | Project root folder create karna | Topics 18–32 | `taskforge-backend/` project root | planned |

### Phase 1 practical output

- [x] Current terminal environment identified
- [ ] Required development tools verified topic-by-topic
- [ ] `taskforge-backend/` project root created at Topic 33
- [ ] Topics 19–33 individually taught and verified

**Phase 1 status: in progress (1/16 topics complete).**
