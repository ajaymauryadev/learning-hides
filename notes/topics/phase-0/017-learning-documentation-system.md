# Topic 17 — Learning documentation system

## Aaj ka exact objective

Aaj durable system define karna hai jisse hundreds of topics ke baad bhi exact
progress, code truth, errors and next step recover ho. Chat memory supporting context
hai; project files authoritative learning state hain.

## Simple definition

**Learning documentation system related files, responsibilities and update rules ka
workflow hai jo batata hai hum kya seekh rahe hain, kya actually implemented/verified
hai, kya errors aaye and next ordered step kya hai.**

```text
Chat ends or context changes
        |
        v
Project files preserve roadmap + evidence + architecture + next topic
        |
        v
New session safely resumes without guessing
```

## Why chat memory enough nahi

- long conversation compact ho sakti hai;
- new chat previous details automatically na rakhe;
- code external changes se chat statement stale ho sakti hai;
- test output and Git diff stronger evidence hain;
- future learner ko files directly inspect karni chahiye.

Rule: **files and verified behaviour are durable truth; chat summary convenience hai.**

## Core documents

### Master plan

`TASKFORGE_BACKEND_MASTER_PLAN.md`: complete 46-phase/1,066-topic curriculum and
teaching contract. Order invent/rearrange nahi hoga.

### Backend roadmap

`notes/BACKEND_ROADMAP.md`: topic number/name/prerequisite/outcome/status. Yeh direction
deta hai, detailed diary nahi.

### Learning state

`notes/LEARNING_STATE.md`: completed topic, files/packages, concepts, flow,
verification, errors, revision and next topic. Yeh resume pointer/history hai.

### Architecture

`notes/ARCHITECTURE.md`: actual current components/relationships. Future concepts only
clearly `planned` label ke saath.

### API contract

`notes/API_CONTRACT.md`: implemented method, URL, auth, params/query/body, success and
error responses, validation, side effects and tests. Endpoint guess record nahi hoga.

### Debug log

`notes/DEBUG_LOG.md`: meaningful incident ka expected result, symptom, layer,
evidence, root cause, smallest fix, retest and prevention.

### Topic lessons

`notes/topics/phase-N/NNN-topic-name.md`: detailed concept, TaskForge examples,
boundaries, verification, exercise and interview answers.

### Per-code-file notes

`notes/each-code-file/`: every important source/config file ki responsibility,
line-by-line explanation, flow, errors, security and verification.

## Before-topic read flow

```text
Inspect files/source/tests/packages
-> read roadmap
-> read learning state
-> read architecture
-> read API contract when relevant
-> read debug log
-> inspect Git status/history
-> select only first incomplete topic
```

## After-topic write flow

```text
Teach/implement one topic
-> run actual relevant verification
-> update file/topic notes
-> sync API/architecture/debug docs if affected
-> mark roadmap only after evidence
-> append learning-state record
-> review Git diff
-> state next topic and stop
```

## Roadmap vs learning state

```text
Roadmap: Where are we? What status is each topic?
Learning state: What happened? What evidence exists? What comes next?
```

Dono same information unnecessarily duplicate nahi karenge.

## Documentation drift

Drift tab hoti hai jab documentation old behaviour describe kare but code/API changed
ho. Prevention:

- code change ke same topic mein docs update;
- tests/actual output before status change;
- stale claim search;
- architecture/API contract compare;
- Git diff mein code plus docs review.

Mismatch mile to docs blindly trust nahi; actual behaviour reproduce/verify karke
correct source of truth establish karo.

## Per-code-file naming

```text
src/config/database.js
-> notes/each-code-file/src.config.database.js.md
```

Flattened name path relationship preserve karta aur nested note tree ko excessive
nahi banata.

Important note covers responsibility, imports/exports, execution, line-by-line syntax,
data flow, success/failure, security, debugging, actual verification, interview and
practice. Full checklist `notes/each-code-file/README.md` mein hai.

## Error documentation flow

```text
Expected -> actual symptom -> error meaning -> failing layer -> hypotheses
-> evidence/isolation -> root cause -> smallest fix -> retest -> prevention
```

Meaningful error silently fix nahi. Har typo ko giant incident bhi nahi banana;
learning/debugging value ke proportional record rakho.

## Verification evidence

Record:

- exact relevant command/check;
- actual pass/fail result;
- success/failure paths;
- what it proves;
- what it does not prove;
- why a check is not applicable.

Never write “all tests pass” unless tests actually executed.

## New-session recovery example

1. Git status shows user changes—preserve them.
2. Roadmap says Topic 72 planned.
3. Learning State says Topic 71 verified, next 72.
4. Architecture/API/debug notes provide current context.
5. Actual files/tests confirm or contradict docs.
6. Resolve contradiction from evidence.
7. Teach only Topic 72.

## Phase folders

Current lessons `notes/topics/phase-0/` mein hain. Next curriculum phase uses
`notes/topics/phase-1/`. Topic number globally ordered rahega; phase folder navigation
easy banata hai.

## Common misconceptions

1. **"Chat mein yaad hai, file unnecessary."** Chat durable source of truth nahi.
2. **"Roadmap and Learning State same hain."** Status index vs evidence/history.
3. **"Architecture future ideal dikhaye."** Actual truth primary; planned clearly label.
4. **"Code changed, docs later."** Later often drift ban jata; same topic mein sync.
5. **"Every source file ko duplicate essay."** Important source/config only; generated
   files proportional explanation.
6. **"Test passed likhna enough."** Actual execution evidence required.

## Verification strategy

Learner ko every core file ka responsibility, before/after update sequence,
roadmap-vs-state difference, drift prevention, flattened naming and new-session resume
flow explain karna aana chahiye.

## Practice exercise

Scenario: Future `src/config/database.js` change hua, connection test failed, then fix
and test pass hua. Kaunsi files update hongi aur kyun?

<details>
<summary>Answer-after-attempt</summary>

- Source file and `notes/each-code-file/src.config.database.js.md`.
- `DEBUG_LOG.md` for meaningful failure/diagnosis/retest.
- `ARCHITECTURE.md` if connection relationship changed.
- `LEARNING_STATE.md` for actual evidence/error/next topic.
- `BACKEND_ROADMAP.md` only after proportional DoD.
- `API_CONTRACT.md` only if observable API behaviour changed.

</details>

## Interview question with Hinglish answer

**Question:** Large learning project mein documentation ko current kaise rakhte ho?

**Answer:** Main roadmap ko ordered status source, Learning State ko chronological
evidence/next-step record, Architecture ko actual relationships, API Contract ko
implemented behaviour and Debug Log ko incident diagnosis ke liye use karta hoon.
Har code topic ke same change mein matching file notes and affected docs sync karta,
actual tests/output record karta and Git diff review ke baad status complete karta hoon.

## Easy-English minimum interview answer

**I keep separate documents for roadmap status, learning evidence, architecture, API
contracts, and debugging history. Before each topic I inspect the current files and
Git state. After implementation I run relevant checks, update all affected documents,
and mark the topic complete only when the evidence satisfies the Definition of Done.**

### Even shorter version

**The project files preserve the roadmap, current architecture, verification evidence,
errors, and next step so work can resume without relying on chat memory.**

## Topic and phase boundary

Topic 17 and Phase 0 are complete after verification. Next is **Phase 1, Topic 18 —
Terminal kya hai?** It has not been started.

