# TaskForge Definition of Done

## Core rule

Task/file ka exist karna “done” nahi. Done tab hai jab current scope clear ho,
required artifact/behaviour present ho, relevant verification actually run ho,
results truthful record hon aur affected documentation synchronized ho.

## Every-topic gate

- Correct roadmap topic and prerequisite selected.
- Only current topic scope covered; future work not silently implemented.
- Concept TaskForge example and boundaries ke saath explained.
- Required artifact created/updated.
- Relevant success and failure understanding/behaviour checked.
- Verification actually performed; unexecuted check passed claim nahi.
- Not-applicable checks explicitly explained.
- Meaningful errors diagnosed and `DEBUG_LOG.md` updated.
- Roadmap/learning state and affected architecture/API/file notes synchronized.
- Practice/revision and interview answers included.
- Git diff/status inspected; secrets/unrelated work protected.
- Next topic stated but not started.

## Code-feature additions

- Syntax/import/package/config/startup checks as applicable.
- API/database success and important failure paths tested.
- Authentication, authorization and side effects verified where relevant.
- Safe errors/logs; secrets/private data excluded.
- Automated regression test added when behaviour warrants it.
- Test evidence says what it proves and does not prove.
- `git diff --check` and scoped review clean.

## Phase completion

- Every phase topic meets its proportional DoD.
- Phase practical output exists and works where executable.
- Cross-topic integration and important regressions verified.
- Documentation describes actual current behaviour.
- Learner can explain phase concepts/flow and complete revision exercise.
- Next phase prerequisites are satisfied.

## Status distinction

- Written: artifact exists.
- Implemented: requested behaviour coded.
- Verified/tested: relevant checks actually passed.
- Done/complete: full current-scope DoD satisfied.
- Deployed: released to an environment; not automatically correct/done.

