# Topic 16 — Definition of Done

## Aaj ka exact objective

Aaj measurable rule define karna hai jisse decide hoga ki topic ya feature genuinely
complete hai. Detailed learning-file workflow Topic 17 mein aayega.

## Simple definition

**Definition of Done (DoD) agreed checklist hai jo batati hai ki work ko complete
kehne se pehle kaunse quality, verification and documentation conditions satisfy
honi chahiye.**

```text
Code/file exists ≠ done
Relevant evidence + correct behaviour + synced docs = done candidate
```

## DoD kyun chahiye?

Without shared DoD, different meanings ho sakte hain:

- developer: “code likh diya”;
- tester: “failure path broken hai”;
- user: “feature usable nahi”;
- operations: “startup fail ho raha hai”.

DoD completion ko feeling se evidence-based decision banata hai.

## Acceptance criteria vs DoD

**Acceptance criteria** specific feature ka expected behaviour batati hain.

```text
Example: valid title par task create ho; missing title reject ho.
```

**Definition of Done** cross-cutting completion quality batati hai.

```text
Tests actually run, safe errors, docs updated, diff reviewed.
```

Dono required ho sakte hain: acceptance criteria answer “kya”; DoD answer “kitni
evidence/quality ke baad complete”.

## Every-topic DoD

1. Correct next roadmap topic selected.
2. Prerequisites/current files/Git state inspected.
3. Topic beginner-friendly TaskForge context mein explained.
4. Only current scope implemented/documented.
5. Required artifact exists.
6. Success and important failure understanding/paths covered.
7. Relevant verification actually run and results reported.
8. Unexecuted test passed claim nahi.
9. Meaningful error evidence-based debug log mein recorded.
10. Roadmap, learning state and affected docs sync.
11. Practice plus Hinglish/easy-English interview answers included.
12. Next topic stated, automatically started nahi.

## Proportional verification

Har topic par same test apply nahi hota.

| Work type | Relevant evidence |
|---|---|
| Concept | Definition, examples, distinctions, exercise, consistency check |
| Documentation | Required sections, truthful links/status, structural check |
| JavaScript function | Syntax/import plus behaviour and edge tests |
| API | Startup, method/path, status/body, success/failure, side effects |
| Database | Connection, persistence, validation/query and failure evidence |
| Security | Allowed plus denied/attack-path regression checks |
| Deployment | Live health, config/log checks and rollback readiness |

“Not applicable” valid ho sakta hai, lekin reason report karna hoga. Conceptual Topic
16 ke liye API startup test pretend karna meaningless hai.

## Code-feature DoD

Future code topic mein as applicable:

- syntax and imports valid;
- exact package/version verified;
- configuration success/failure checked;
- app/database starts correctly;
- API contract implemented;
- success and important failure paths tested;
- authentication/authorization/ownership checked;
- expected side effect occurred and forbidden side effect did not;
- safe error/log output; no secret leak;
- automated regression coverage;
- affected file notes/API/architecture sync;
- diff review and `git diff --check` clean.

## Example: create task

Incomplete evidence:

```text
Controller file exists.
Manual success once appeared.
```

Stronger DoD evidence:

```text
Valid authorized input -> one correct task stored -> expected safe response
Missing title -> validation error -> no task stored
Unauthorized user -> denied -> no task stored
Database failure -> safe error -> no false success
Tests executed and output recorded
API contract and learning/file notes synchronized
Diff reviewed; no secrets/unrelated overwrite
```

## Done vs deployed

- Done feature local/test evidence satisfy kar sakta hai but deployed na ho.
- Deployed code environment mein release ho sakta hai but tests/docs/security missing
  hone par DoD satisfy na kare.

Deployment location hai; done quality/evidence decision hai.

## Phase DoD

Phase complete when:

- every included topic complete;
- practical phase output present/working;
- combined flow/integration verified where applicable;
- docs current behaviour reflect karti hain;
- learner explanation/revision perform kar sakta hai;
- next phase prerequisites satisfied.

Phase 0 ko Topic 17 aur phase-level verification ke baad complete kahenge.

## Reopen completed work

“Complete” permanent immunity nahi. Regression, incorrect evidence or changed
requirement mile to status reopen/update ho sakta hai. Honest state false green status
se more important hai.

## Common misconceptions

1. **"Code compile/run hua, done."** Behaviour, failures, security and docs missing ho
   sakte hain.
2. **"100% tests pass means done."** Tests incomplete/wrong ho sakte; acceptance and
   other evidence required.
3. **"Manual happy path enough."** Failure and side-effect checks bhi matter.
4. **"Deployed means done."** Broken/unsafe undocumented code deploy ho sakta hai.
5. **"Har topic ko API test chahiye."** Verification proportional honi chahiye.
6. **"Done status kabhi change nahi hota."** Regression/new evidence reopen kar sakta.

## Verification strategy

Learner ko DoD define, acceptance criteria se distinguish, proportional evidence
choose, feature-done and deployed separate, and create-task failure/side effects
explain karna aana chahiye.

## Practice exercise

Statement: “Login endpoint done hai because valid password se token mila.”

Missing DoD evidence list karo:

```text
Invalid credentials:
Missing input:
Sensitive response/log:
Token configuration:
Automated regression:
Documentation:
Side effects:
Diff/security review:
```

<details>
<summary>Answer-after-attempt</summary>

Invalid/missing credentials failure, generic safe error, password/token redaction,
expiry/secret configuration, automated success/failure tests, API/file notes, session
side effects and scoped diff/security review still verify karne honge.

</details>

## Interview question with Hinglish answer

**Question:** Definition of Done kya hai aur aap feature ko done kab bolte ho?

**Answer:** DoD agreed quality checklist hai. Main feature ko tab done bolta hoon jab
acceptance criteria implemented hon, relevant success/failure and side effects
actually verify hon, security/error handling correct ho, automated regression test
where applicable pass ho, documentation sync ho and diff review clean ho. Sirf code
likhna ya one happy-path response enough nahi.

## Easy-English minimum interview answer

**Definition of Done is a shared checklist that work must satisfy before it is called
complete. For a backend feature, I verify the acceptance criteria, success and
important failure paths, side effects, security, tests, documentation, and code diff.
I never call an unexecuted test passed.**

### Even shorter version

**Work is done only when the required behaviour is implemented, verified with
evidence, and documented—not merely when the code is written.**

## Topic boundary

Topic 16 mein Definition of Done complete hui. **Learning documentation system**
Topic 17 ko iske baad separately complete kiya gaya.
