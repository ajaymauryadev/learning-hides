# TaskForge Backend Learning Memory

## Student profile

- Student ko sirf basic JavaScript aati hai.
- Node.js, Express, HTTP, APIs, MongoDB, Mongoose, architecture, testing, authentication, security, deployment aur system design ko beginner level se samjhana hai.
- Teaching language simple Hinglish hogi, lekin technical terms correct English mein rahengi.
- Kisi concept ko pehle dikh chukne ke karan understood assume nahi karna hai.
- Final target sirf project complete karna nahi, balki lagbhag 3-year backend developer ke barabar practical capability develop karna hai.
- Experience ka false promise nahi karna: capability implementation, debugging, testing, repetition, code reading, refactoring, interviews aur independent exercises se build hogi.

## Canonical project and roadmap

- Ek hi evolving learning project: **TaskForge — Team Project, Task and Issue Management Backend**.
- Backend API pehle banegi; frontend baad mein optional hai.
- Canonical complete curriculum aur teaching contract `TASKFORGE_BACKEND_MASTER_PLAN.md` mein verbatim saved hai.
- Curriculum mein 46 phases aur 1,066 ordered topics hain.
- Order kabhi skip, reorder ya independently invent nahi karna.
- Ek topic complete aur actually verify hone ke baad hi next topic par jaana hai.
- Ek turn/chunk mein sirf ek topic teach/implement karna hai; poora phase ya future code dump nahi karna.

## Required teaching behaviour

- Har topic se pehle actual project files, roadmap, learning state, architecture, relevant API contract, debug log aur Git status inspect karna hai.
- Pehle concept, TaskForge mein need, prerequisites, involved files/packages, data/control flow, success/failure paths aur verification samjhani hai.
- Har new/unfamiliar meaningful JavaScript line ke immediately upar simple Hinglish comment dena hai.
- Important syntax, operators, parameters, arguments, return values, callbacks aur Promises explain karne hain.
- Meaningful error silently fix nahi karna; evidence-based diagnosis aur `notes/DEBUG_LOG.md` update karna hai.
- Tests/verification ko actually run karna hai; unexecuted test ko passed kabhi claim nahi karna.
- Har important source/config file ke liye `notes/each-code-file/` mein matching detailed Markdown note maintain karna hai.
- Code ke saath roadmap, learning state, architecture, API contract aur debug documentation sync rakhni hai.
- Secrets, tokens, password/hash, database URI ya private data kabhi expose nahi karna.
- Architecture modular monolith se start hogi; microservices, Redis, queues, Docker ya Kubernetes sirf real need/learning stage par aayenge.
- Har topic ke baad relevant interview questions with answers, practice/revision, Git commands aur sirf next topic ka naam dena hai; next topic automatically start nahi karna.
- Har topic ke end mein Hinglish explanation ke saath ek short **easy-English minimum interview-ready answer** bhi dena hai, jise beginner naturally bol sake.
- Bahut saare phases ko manageable rakhne ke liye topic lessons official curriculum numbering ke phase-wise folders mein store karne hain. Current path `notes/topics/phase-0/` hai.
- Guidance gradually kam hogi: teacher-led -> together -> fill missing code -> debug -> independent feature.

## Durable progress files

TaskForge initialize hone par ye files create aur maintain karni hain:

```text
notes/
|-- BACKEND_ROADMAP.md
|-- LEARNING_STATE.md
|-- ARCHITECTURE.md
|-- API_CONTRACT.md
|-- DEBUG_LOG.md
`-- each-code-file/
```

Chat memory par depend nahi karna. `BACKEND_ROADMAP.md` direction dega aur `LEARNING_STATE.md` exact progress batayega.

## Current state

- Master plan read and saved.
- TaskForge application abhi initialize nahi hui hai.
- Phase 0 ke Topics 1–17 aur Phase 1 ke Topics 18–33 complete/locally verified hain.
- Phase 1 practical output `taskforge-backend/` exact workspace child ke roop mein exists.
- Folder empty hai, parent `Practicle` Git work tree ke andar hai, own `.git` nahi.
- Phase 2 Topic 34 complete: version-control mental model established; no Git mutation.
- Phase 2 Topic 35 complete: Git distributed VCS mental model established; no mutation.
- Phase 2 Topic 36 complete: current non-bare parent repository boundary verified.
- Phase 2 Topic 37 complete: working-tree and three-state mental model established.
- Phase 2 Topic 38 complete: untracked-file lifecycle and safety established.
- Phase 2 Topic 39 complete: tracked-file identity and state dimensions established.
- Phase 2 Topic 40 complete: staging/index proposed-snapshot model established.
- Phase 2 Topic 41 complete: commit object/snapshot/history model established.
- Phase 2 Topic 42 complete: branch/ref/HEAD and divergence model established.
- Phase 2 Topic 43 complete: remote/upstream/cached-reference model established.
- Phase 2 Topic 44 complete: safe repository initialization and nested-boundary model established.
- Phase 2 Topic 45 complete: long/short/porcelain repository-status model established.
- Phase 2 Topic 46 complete: TaskForge ignore rules and secret-prevention limits established.
- Phase 2 Topic 47 complete: exact-path staging and content-snapshot timing established.
- Phase 2 Topic 48 complete: unstaged diff boundary and patch-reading model established.
- Phase 2 Topic 49 complete: staged/cached diff and pre-commit review model established.
- Phase 2 Topic 50 complete: reviewed staged snapshot recorded as a local commit.
- Phase 2 Topic 51 complete: commit-history navigation and filtering model established.
- Phase 2 Topic 52 complete: remote name/URL/config inspection model established.
- Phase 2 Topic 53 complete: verified normal fast-forward publication to `origin/main` established.
- Phase 2 Topic 54 complete: fetch-plus-integration pull model and `--ff-only` safety established.
- Phase 2 Topic 55 complete: repeatable inspect-review-stage-commit-sync-verify workflow established.
- Current curriculum phase: Phase 2 — Git aur repository foundation.
- Current topic lesson folder: `notes/topics/phase-2/`.
- Next authorized learning step: **Phase 2, Topic 56 — Secrets ko Git se bachana.**
- Topic 56 tabhi start karna hai jab student next topic ke liye kahe.
