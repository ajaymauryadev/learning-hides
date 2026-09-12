# TaskForge Learning Documentation System

## Purpose

Chat context temporary ho sakta hai. Project files durable source of truth hain. Har
session actual files padhkar exact next topic, architecture and evidence resume karega.

## Authority order

1. `TASKFORGE_BACKEND_MASTER_PLAN.md`: complete canonical curriculum/teaching contract.
2. `notes/BACKEND_ROADMAP.md`: ordered topic status and immediate direction.
3. `notes/LEARNING_STATE.md`: completed work, evidence, errors, revision and next topic.
4. Actual source/config/tests and Git diff: implemented truth.
5. `notes/ARCHITECTURE.md` and `notes/API_CONTRACT.md`: current relationships/behaviour.
6. `notes/DEBUG_LOG.md`: preserved incident diagnosis.
7. Topic and per-code-file notes: detailed learning explanations.

When prose conflicts with actual verified code/test output, claim completion mat karo;
evidence inspect, discrepancy debug and documents correct karo.

## Before every topic

1. Inspect actual files/source/tests/packages.
2. Read roadmap and learning-state current position.
3. Read architecture, relevant API contract and debug log.
4. Check Git status/recent history; preserve unrelated changes and secrets.
5. Select only first incomplete ordered topic.
6. Explain objective, need, prerequisites, files/packages, flow, failures and verification.

## During topic

- One small practical chunk only.
- New meaningful JavaScript lines get immediate simple Hinglish comments.
- Meaningful errors are not silently fixed; evidence-based debug lifecycle is recorded.
- Never expose credentials, tokens, private data or production internals.
- Do not create future architecture/layers without current need.

## After topic

1. Run proportional verification; record actual results and limitations.
2. Update matching topic note and per-code-file notes.
3. Update API contract if behaviour changed.
4. Update architecture if relationships changed.
5. Update debug log if meaningful incident occurred.
6. Update roadmap status only after verification.
7. Append learning-state record and next topic.
8. Inspect Git diff/status and provide safe scoped commands.
9. State next topic, do not start it.

## Document responsibilities

- `BACKEND_ROADMAP.md`: phase/topic/name/prerequisite/outcome/status; not a diary.
- `LEARNING_STATE.md`: chronological evidence/history and resume pointer.
- `ARCHITECTURE.md`: actual current structure plus clearly labelled planned concepts.
- `API_CONTRACT.md`: implemented API method/path/input/output/errors/side effects/tests.
- `DEBUG_LOG.md`: expected, symptom, layer, evidence, cause, fix, retest, prevention.
- `PRODUCT_DEFINITION.md`: stable product problem/scope/current truth.
- `USERS_AND_ROLES.md`: actor and authority-scope definitions.
- `USE_CASES.md`: actor goals, flows, outcomes and state expectations.
- `SYSTEM_DIAGRAM.md`: high-level component/flow view.
- `DEFINITION_OF_DONE.md`: completion evidence gate.
- `DEVELOPMENT_PHASES.md`: dependency-based milestone sequence.
- `topics/phase-N/`: topic-specific lesson, exercise and interview answers.
- `each-code-file/`: important source/config-file teaching notes.

## Status transition rule

Possible states: `planned`, `learning`, `implemented`, `locally verified`, `tested`,
`revised`, `complete`. Not every conceptual topic needs implementation/test states.
`complete` requires its proportional Definition of Done; never infer it from file
existence alone.

## Resume algorithm

```text
Open project
-> inspect Git/files
-> read roadmap current statuses
-> read learning-state next topic/evidence
-> inspect architecture/API/debug context
-> resolve inconsistencies from actual evidence
-> teach only first incomplete topic
```

