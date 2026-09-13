# Topic 55 — Safe Git workflow

## Learning goal

Topics 34–54 ke separate commands ko one repeatable daily workflow mein जोड़ना है। Goal सिर्फ
commands याद करना नहीं; हर mutation से पहले सही question/evidence check करना है। Topic 54–55 docs
पर complete inspect-to-push workflow practically verify करना है।

## Sabse simple workflow

```text
1. कहाँ हूँ?              -> location/repository/branch check
2. क्या बदला है?          -> status + diff
3. remote से update?      -> clean state + fetch/pull strategy
4. क्या select करना है?   -> exact git add
5. commit में क्या जाएगा? -> cached diff
6. record सही है?         -> commit + log/status
7. कहाँ भेजना है?         -> remote/live state check
8. publish हुआ?           -> push + verify
```

Short memory:

> **Inspect → Update → Edit → Review → Stage → Re-review → Commit → Sync → Verify**

## Parcel wala easy example

Imagine courier भेजना है:

```text
घर/address verify       = repository root + remote URL check
सामान देखें             = status + diff
selected सामान pack करें = git add
sealed box दुबारा देखें  = git diff --cached
label/receipt बनाएँ      = git commit
courier को दें          = git push
delivery confirm करें    = remote verification
```

अगर box review किए बिना courier कर दिया, wrong या private item बाहर जा सकती है। Git में staged diff
और remote destination review उसी safety gate का काम करते हैं।

## Golden rule

**Read-only evidence पहले, mutation बाद में।**

```text
status before add
cached diff before commit
remote/branch before push
graph/state before pull integration
```

Mutation command successful होना intended outcome prove नहीं करता; post-command verification भी
required है।

## Step 1 — Correct location and repository

```powershell
Get-Location
git rev-parse --show-toplevel
git branch --show-current
git status --short --branch
```

Questions:

- क्या मैं intended workspace में हूँ?
- Repository root कौनसा है?
- Current branch सही है?
- Working tree clean/dirty क्यों है?

Current repository root parent `Practicle` है; `taskforge-backend` child का own `.git` नहीं।

## Step 2 — Remote destination inspect

```powershell
git remote -v
git remote get-url --push origin
git rev-parse --abbrev-ref "main@{upstream}"
```

Questions:

- Push किस owner/repository पर जाएगी?
- Fetch और push URLs expected हैं?
- URL में embedded secret तो नहीं?
- Current branch का upstream क्या है?

Remote name देखकर destination assume नहीं करना।

## Step 3 — Existing work protect karo

```powershell
git status --short
git diff --name-status
git diff
git diff --check
```

Before update/pull:

- uncommitted work समझो;
- unrelated changes अलग रखो;
- accidental deletion और generated files पहचानो;
- secrets/private data look for करो;
- unclear work को discard मत करो।

Current Topic 55 baseline में 4 tracked docs modified, Topic 54 lesson untracked और staged count zero
था। यह expected previous lesson work था।

## Step 4 — Remote update strategy

Simple safe aligned case:

```powershell
git pull --ff-only origin main
```

Team workflow में inspect-first approach:

```powershell
git fetch origin
git log --graph --oneline --decorate --all
```

Then merge/rebase/fast-forward strategy consciously choose करो। Dirty work, divergence या unclear
incoming changes पर blind pull मत करो। Current safe-workflow practical में live remote tip पहले local
HEAD/cached ref से match verify हुई।

## Step 5 — Edit/build/test

Application phase में:

```text
small coherent change बनाओ
  -> relevant automated tests run करो
  -> lint/format/type/security checks जो project में exist हों run करो
  -> runtime/manual behavior verify करो
```

Abhi TaskForge application initialize नहीं है, इसलिए application tests/package scripts available
नहीं। Documentation structure, Git diffs और whitespace checks ही relevant verification हैं। Tests
न होने पर “tests passed” claim नहीं करना।

## Step 6 — Review before staging

```powershell
git status --short
git diff --stat
git diff -- exact/path
git diff --check
```

Review questions:

- Change requirement पूरा करता है?
- कोई unrelated edit है?
- secret/token/password/private data है?
- generated dependency/output mistakenly included है?
- debug code/temporary file बची है?
- documentation/tests update required हैं?

## Step 7 — Exact staging

```powershell
git add -- exact/file-one exact/file-two
```

Prefer exact paths। Use broad commands only after full scope review:

```powershell
git add .
git add -A
```

Broad commands गलत नहीं, लेकिन उनका selection area बड़ा है। Conflict-example correction ka debug
record include karne ke baad current practical mein exactly seven
Topic 54–55 learning paths selected हुए।

## Step 8 — Re-review proposed commit

```powershell
git status --short
git diff --cached --name-status
git diff --cached --stat
git diff --cached
git diff --cached --check
```

Most important question:

> “अगर अभी commit करूँ, तो exactly यही coherent content जाना चाहिए?”

Wrong path/secret/incomplete content मिले तो commit रोक दो।

## Step 9 — Meaningful local commit

```powershell
git commit -m "docs: complete phase 2 topics 54 and 55"
```

Good commit:

- one coherent purpose;
- reviewed staged snapshot;
- meaningful message;
- no secret/unrelated generated files;
- relevant checks performed।

After commit:

```powershell
git status --short --branch
git log -1 --oneline
git show --stat --oneline HEAD
```

## Step 10 — Safe remote publication

Before push:

```powershell
git remote -v
git status --short --branch
git log --oneline --decorate -5
```

Remote state may change between checks (race possible). Normal push rejection is safety signal;
force से override नहीं करना।

Publish:

```powershell
git push origin main
```

Current practical expected normal fast-forward only; no force option allowed।

## Step 11 — Post-push verification

```powershell
git status --short --branch
git rev-parse main
git rev-parse origin/main
git log -1 --oneline --decorate
```

Optional live remote evidence:

```powershell
git ls-remote --heads origin refs/heads/main
```

Expected successful aligned result:

```text
working tree clean
main == origin/main == live remote main
ahead/behind 0/0
```

## One complete command sequence

यह blindly paste करने का script नहीं; हर arrow पर output समझना है:

```powershell
Get-Location
git rev-parse --show-toplevel
git status --short --branch
git remote -v
git diff --name-status
git diff
git diff --check
git add -- exact/path-one exact/path-two
git diff --cached --name-status
git diff --cached
git diff --cached --check
git commit -m "clear message"
git status --short --branch
git log -1 --oneline
git push origin main
git status --short --branch
```

## Decision gates

| Gate | Stop कब करना है? | Next safe action |
|---|---|---|
| Location | wrong root/branch | correct location; mutation मत करो |
| Working state | unknown/unrelated changes | owner/content inspect and preserve |
| Pull/update | dirty state or divergence unclear | fetch/graph/team strategy |
| Staging | secret/generated/wrong paths | selection और ignore policy fix |
| Commit | cached diff incomplete | edit/re-stage/re-review |
| Push | wrong URL, remote changed, rejection | stop, inspect, coordinate |
| Post-push | refs/output mismatch | investigate; success assume मत करो |

## Personal work versus team work

Solo repository में भी safe workflow useful है। Team में extra rules हो सकती हैं:

- feature branch;
- pull request/review;
- protected `main`;
- required CI checks;
- signed commits;
- issue/ticket reference;
- release/deployment approval।

Current repository direct `main` push allow करती है, लेकिन future workplace policy अलग हो सकती है।

## Never-do-without-understanding list

```text
git reset --hard
git clean -fd
git push --force
git checkout -- <path>
git restore <path> (discard form)
history rewrite on shared branch
```

इन commands के legitimate uses हो सकते हैं, but they can discard/overwrite work. Exact target,
recovery path और team impact जाने बिना नहीं चलाना।

## Recovery mindset

Mistake होने पर panic में multiple destructive commands मत चलाओ:

```text
1. Stop
2. Current status/output copy करो
3. Exact command and affected paths identify करो
4. Local/remote/committed/uncommitted state separate करो
5. Recovery options and team impact assess करो
6. Smallest safe correction करो
7. Verify
```

Error output evidence है, shame नहीं।

## Data/control flow

```text
inspect local state
  -> safely learn remote state
  -> edit and verify
  -> exact index selection
  -> cached review
  -> local commit
  -> remote fast-forward publication
  -> local/live verification
```

Git workflow source history manage करता है। Application tests, database migration और deployment
separate gates हैं जिन्हें future phases में जोड़ेंगे।

## Common errors aur easy fixes

### Wrong folder में command चली

Mutation रोकें। `Get-Location` और `git rev-parse --show-toplevel` verify करें। Nested/parent boundary
समझे बिना init/add/commit मत करें।

### Accidental broad staging

Commit मत करें। Cached name/status और diff inspect करें; working content preserve रखते हुए unwanted
index selection safely correct करें, then exact paths add करें।

### Commit में secret दिखा

Push रोकें। Topic 56 में complete incident workflow आएगा। Credential exposure possible हो तो rotate
करना सिर्फ file हटाने से ज्यादा important है।

### Pull ने conflict दिया

Random commands मत चलाएँ। Both versions और intended behavior समझें, conflict resolve, tests और diff
review करें। Team strategy follow करें।

### Push non-fast-forward reject हुई

यह safety protection है। Remote updates fetch/inspect/integrate करें। Force push routine fix नहीं।

### Commit/push successful, tests नहीं चले

Git success behavior correctness prove नहीं करता। Available relevant tests/checks separately run और
evidence record करें। Application absent हो तो साफ बताएं कि tests applicable नहीं थे।

### Working tree clean को deployment success समझा

Clean केवल Git comparison state है। Remote, CI और deployed runtime अलग systems हैं।

## Student exercise

1. Safe workflow का first step क्या है?
2. Commit से ठीक पहले कौनसा content review जरूरी है?
3. Exact `git add` broad add से safer क्यों है?
4. Push से पहले क्या verify करोगे?
5. Non-fast-forward rejection पर क्या नहीं करना चाहिए?
6. Git status clean क्या tests pass prove करता है?

## Exercise answers

1. Location, repository root, branch और status inspect करना।
2. `git diff --cached` से proposed commit content.
3. Selection scope controlled है और unrelated paths का risk कम है।
4. Clean/expected state, branch, push URL, commits और remote live/cached relation.
5. Blind force push नहीं करना।
6. नहीं, Git state और application behavior अलग हैं।

## Interview question with Hinglish answer

**Question:** आपका safe Git workflow क्या है?

**Answer:** मैं पहले repository root, branch, status और remote verify करता हूँ। Changes को plain diff से
review करके exact paths stage करता हूँ, फिर cached diff और checks से proposed commit दुबारा inspect
करता हूँ। Meaningful local commit के बाद remote state/destination verify करके normal push करता हूँ और
अंत में status, log और refs से result confirm करता हूँ। Rejection या unknown changes पर force/destructive
commands नहीं चलाता।

## Easy-English minimum interview answer

> My safe Git workflow is: verify the repository and branch, inspect status and unstaged changes,
> stage exact files, review the staged diff, run relevant checks, create a meaningful commit, verify
> the remote destination and state, push normally, and confirm the final status and references. I
> stop and investigate unexpected changes or rejections instead of forcing the operation.

## Completion boundary

Topic 55 में Topic 54–55 documentation पर complete safe workflow verified हुआ: exact inspection,
staging, cached review, local commit, normal fast-forward push और post-push alignment. No force,
destructive recovery, pull conflict, application code या fake test claim हुआ। Topic 56 — Secrets को
Git से बचाना अभी start नहीं हुआ।
