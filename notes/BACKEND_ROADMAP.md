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
| 9 | Source code aur running process ka difference | Topic 8 | File aur active program ka difference | planned |
| 10 | Local development aur production ka difference | Topics 8–9 | Environments ka basic distinction | planned |
| 11 | TaskForge product overview | Topics 1–10 | Product scope ka detailed overview | planned |
| 12 | TaskForge ke users | Topic 11 | User types ka definition | planned |
| 13 | Primary use cases | Topics 11–12 | Main user goals/actions | planned |
| 14 | High-level system diagram | Topics 2–13 | Initial system relationships | planned |
| 15 | Initial development phases | Topics 1–14 | Implementation sequence ka overview | planned |
| 16 | Definition of Done | Topic 15 | Completion criteria | planned |
| 17 | Learning documentation system | Topics 1–16 | Durable learning workflow | planned |

### Phase 0 practical output

- [x] TaskForge product definition ka initial foundation
- [x] Backend roadmap/progress tracker initialized
- [x] Honest current architecture document initialized
- [x] Learning-state document initialized
- [ ] Topics 9–17 individually taught and verified
