# Topic 53 — `git push`

## Learning goal

Simple example se local commits ko remote repository par safely publish karna samajhna hai. Push से
पहले remote destination, live remote tip, staged/committed state और fast-forward relationship verify
करना है। Force push नहीं करना।

## Sabse simple definition

**`git push` local repository ke commits/objects ko remote repository par bhejta hai aur allowed
hone par remote branch reference update karta hai.**

```text
local main commits
      | git push origin main
      v
remote repository main branch
```

Push working files को directly upload नहीं करता; केवल committed Git history publish करता है।

## Notebook aur shared drive wala easy example

Imagine:

```text
Laptop par saved document version = local commit
Shared drive wali copy             = remote branch
Upload/sync saved version          = git push
```

Rough unsaved edits यानी unstaged/uncommitted changes push नहीं होंगी। पहले meaningful local commit
बनना चाहिए।

## Practical command

```powershell
git push origin main
```

Command anatomy:

```text
git     -> Git program
push    -> publish/update remote refs subcommand
origin  -> configured remote name
main    -> local source branch; simple form में remote main target
```

Command run करने से पहले `git remote -v` से push URL verify करना जरूरी है।

## Topic start ki verified state

```text
local HEAD:        fdf6d79...
cached origin/main: a5a3b51...
live remote main:   a5a3b51...
local vs remote:    ahead 1, behind 0
```

Live remote `git ls-remote --heads origin refs/heads/main` से read-only check हुई। It matched cached
`origin/main`, इसलिए कोई unseen remote commit नहीं मिली और normal fast-forward publish safe था।

## Push se pehle current docs commit

Topics 51–53 files और trackers पहले exact paths से stage/review करके local commit में record हुए:

```powershell
git add -- LEARNING_MEMORY.md notes/ARCHITECTURE.md notes/BACKEND_ROADMAP.md notes/LEARNING_STATE.md notes/topics/phase-2/051-git-log.md notes/topics/phase-2/052-git-remote.md notes/topics/phase-2/053-git-push.md
git diff --cached --name-status
git diff --cached --check
git commit -m "docs: complete phase 2 topics 51 to 53"
```

Then that new local commit सहित पहले से ahead commit remote पर publish हुआ।

## Commit aur push ka difference

| Command | कहाँ change करता है? | Main purpose |
|---|---|---|
| `git commit` | local repository | staged snapshot को local history बनाना |
| `git push` | remote repository और local remote-tracking update | local commits publish करना |

```text
edit -> add -> commit -> push
```

इन steps को same action मत समझो।

## Push commits करता है, files नहीं

Example:

```text
Commit A: remote पर already है
Commit B: local only
Commit C: local only
Uncommitted file edit: working tree only
```

Successful push after B/C:

```text
remote receives required objects for B and C
remote main moves from A to C
uncommitted edit remains only working tree
```

## Fast-forward push

Safe normal case:

```text
remote: A
local:  A -> B -> C
```

Remote A local history का ancestor है, so remote branch can move forward to C without losing its
known commits. इसे fast-forward update कहते हैं।

## Non-fast-forward rejection

Conflict case:

```text
remote: A -> X
local:  A -> B
```

Remote पर X है जो local history में नहीं। Normal push reject हो सकती है:

```text
rejected (non-fast-forward)
```

Correct response: force push नहीं। पहले remote changes fetch/inspect/integrate करो, tests/review करो,
फिर normal push try करो। Pull Topic 54 में आएगा।

## `-u` / `--set-upstream`

New branch के first push पर:

```powershell
git push -u origin feature-name
```

`-u` successful push के साथ local branch का upstream tracking set कर सकता है। बाद में simple
`git push` possible हो सकता है। Current `main` का upstream already `origin/main` था, इसलिए `-u`
required नहीं था।

## Explicit form safer kyun?

```powershell
git push origin main
```

Beginner के लिए destination intention visible है। Bare `git push` behavior upstream और
`push.default` configuration पर depend कर सकता है। फिर भी explicit command से पहले URL inspect
करना जरूरी है; name `origin` ownership prove नहीं करता।

## Authentication versus authorization

```text
authentication -> server जानता है आप कौन हैं
authorization  -> server decide करता है आपको main push permission है या नहीं
```

Correct login के बाद भी protected branch policy push रोक सकती है। Some teams require pull request
और direct `main` push deny करती हैं।

## Push output ka easy meaning

Typical success:

```text
To https://host/owner/repository.git
   oldHash..newHash  main -> main
```

- destination URL shown है;
- remote main old commit से new commit पर moved;
- `main -> main` local source और remote destination बताता है।

`Everything up-to-date` means specified refs के लिए publish करने को नया commit नहीं; यह working
tree clean या tests pass होने का proof नहीं।

## Remote-tracking ref after push

Successful push के बाद Git local `origin/main` को accepted remote result तक update कर सकता है:

```text
main -> new commit
origin/main -> same new commit
ahead/behind -> 0/0
```

Live remote verification separate read से confirm की जा सकती है।

## Tags automatically?

Normal branch push every local tag automatically publish करे, यह assume मत करो। Tags के explicit
push options अलग हैं:

```powershell
git push origin tag-name
git push origin --tags
```

`--tags` broad external mutation है; current lesson में tags push नहीं हुए।

## Force push risk

```powershell
git push --force
```

Remote branch history overwrite/loss कर सकता है और teammates का work break हो सकता है। Current
main पर force push बिल्कुल नहीं किया। Even `--force-with-lease` advanced safeguard है, routine fix
नहीं। Rejection का root cause पहले understand करो।

## Data/control flow

```text
local source ref resolve
  -> remote URL connect
  -> authenticate/authorize
  -> compare remote and local history
  -> missing objects transfer
  -> server policies/hooks check
  -> remote ref update accepted/rejected
  -> local tracking info update
```

Push TaskForge application deploy या database migrate नहीं करता।

## Safe push workflow

```text
git status --short --branch
  -> tests/checks (when application exists)
  -> git log --oneline --decorate -5
  -> git remote -v
  -> live/cached remote state inspect
  -> confirm ahead/behind and destination
  -> git push origin main
  -> inspect output/exit code
  -> verify local origin/main and live remote main
```

## Common errors aur easy fixes

### `rejected (non-fast-forward)`

Remote has commits/history not safely contained in local tip. Fetch/pull strategy, inspect and
integrate; force push default fix नहीं।

### `Authentication failed`

Credential missing/expired/wrong हो सकती है। Secure credential manager/SSH setup और account verify
करो; token URL में paste मत करो।

### `Permission denied` / protected branch

Identity authenticated हो सकती है but branch/repository write authorization नहीं। Correct remote,
team policy और pull-request workflow verify करो।

### `src refspec ... does not match any`

Branch name wrong हो सकता है या fresh repository में commit नहीं। `git branch --show-current` और
`git log -1` inspect करो।

### Wrong repository par push ho gaya

Pre-push URL/owner verification miss हुई। तुरंत team/repository owner को बताओ; secrets हों तो rotate
करो। History deletion बिना coordination मत करो।

### Push success but uncommitted code absent

Expected: push commits publish करता है, working-tree edit नहीं। Status inspect, review, add और new
commit बनानी होगी।

### `Everything up-to-date` but GitHub content old लगता है

Wrong branch/remote/view हो सकता है, या change commit नहीं हुई। Local HEAD, branch, remote URL और
remote selected branch inspect करो।

## Student exercise

1. Push क्या publish करता है?
2. क्या uncommitted file push होती है?
3. `origin` और `main` command में क्या हैं?
4. Fast-forward push का simple meaning क्या है?
5. Non-fast-forward rejection पर force push करना चाहिए?
6. Commit और push में difference क्या है?

## Exercise answers

1. Local committed Git objects/history और intended remote ref update.
2. नहीं।
3. `origin` remote name, `main` branch/ref name.
4. Remote tip local history का ancestor है; branch safely आगे move कर सकती है।
5. नहीं; remote changes inspect/integrate करो।
6. Commit local history बनाता है; push history remote पर publish करता है।

## Interview question with Hinglish answer

**Question:** `git push` क्या करता है?

**Answer:** `git push` local commits और required Git objects remote repository को भेजता है और server
allow करे तो remote branch ref update करता है। यह uncommitted working files नहीं भेजता। Push से पहले
मैं destination URL, branch, ahead/behind और cached/live remote state check करता हूँ; non-fast-forward
rejection पर force push नहीं करता।

## Easy-English minimum interview answer

> `git push` sends local commits and required Git objects to a remote repository and requests a
> remote branch update. It does not upload uncommitted working-tree changes. I verify the destination
> and remote state before pushing, and I do not use force push as a routine fix.

## Completion boundary

Topic 53 में Topics 51–53 documentation local commit बनाकर normal fast-forward से `origin/main` पर
publish हुई। Live remote tip, local tracking ref और `0/0` alignment verify हुए। No force push, pull,
reset, amend, tag push या application change हुआ। Topic 54 — `git pull` अभी start नहीं हुआ।
