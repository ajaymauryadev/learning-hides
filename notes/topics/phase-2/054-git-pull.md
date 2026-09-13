# Topic 54 — `git pull`

## Learning goal

Easy examples se remote changes local branch mein lana samajhna hai. Pull ke two parts—fetch और
integration—clear करने हैं। Clean state, explicit remote/branch और `--ff-only` के साथ safe no-op pull
verify करनी है।

## Sabse simple definition

**`git pull` pehle remote se latest known history fetch karta hai, phir fetched branch ko current
local branch mein integrate karta hai.**

```text
git pull = git fetch + integration
```

Integration configuration/command के हिसाब से merge या rebase हो सकती है। इसे सिर्फ “download” मत
समझो; यह current branch बदल भी सकता है।

## Shared document wala easy example

Imagine team shared document:

```text
Step 1: server se latest copy lao       = fetch
Step 2: apni current copy se combine करो = merge/rebase integration
Both steps together                     = pull
```

इसलिए pull, fetch से ज्यादा powerful और risky है। Fetch information लाता है; pull fetched work को
current branch में integrate करने की कोशिश भी करता है।

## Safe practical command

```powershell
git pull --ff-only origin main
```

Command anatomy:

```text
git       -> Git program
pull      -> fetch then integrate command
--ff-only -> only fast-forward allow; divergence पर stop
origin    -> remote name
main      -> remote branch to fetch/integrate
```

Current practical output:

```text
From https://github.com/ajaymauryadev/learning-hides
 * branch main -> FETCH_HEAD
Already up to date.
```

Meaning:

- Git ने remote `main` query/fetch किया;
- fetched result `FETCH_HEAD` में temporarily refer हुआ;
- local `main` में कोई missing remote commit नहीं थी;
- file content और HEAD change नहीं हुए।

## Topic start ki verified state

Pull से पहले:

```text
working tree: clean
current branch: main
upstream: origin/main
ahead/behind: 0/0
HEAD: 4320eb8...
```

Pull के बाद:

```text
HEAD: same 4320eb8...
origin/main: same as HEAD
ahead/behind: 0/0
working tree: clean
```

यह no-op integration है, लेकिन network fetch operation actually हुई।

## Fetch, pull aur push difference

| Command | Direction | Main effect |
|---|---|---|
| `git fetch` | remote → local metadata/objects | remote history लाता है; current branch automatically integrate नहीं करता |
| `git pull` | remote → local + integration | fetch करता है, then current branch update/combine कर सकता है |
| `git push` | local → remote | local commits remote branch पर publish करता है |

Easy memory:

```text
fetch = देखो/लाओ
pull  = लाओ + मिलाओ
push  = भेजो
```

## Fast-forward case

```text
local:  A
remote: A -> B -> C
```

Local के पास unique commit नहीं। `--ff-only` local branch pointer A से C तक safely आगे move कर सकता
है:

```text
local after pull: A -> B -> C
```

No merge commit needed।

## Already up-to-date case

```text
local:  A -> B
remote: A -> B
```

Nothing new integrate करना है:

```text
Already up to date.
```

यह current practical था।

## Local ahead case

```text
local:  A -> B -> C
remote: A -> B
```

Remote से missing commit नहीं; pull often no integration change करेगा। Local C publish करने के लिए
later push चाहिए। Pull local commit को remote पर नहीं भेजता।

## Diverged case

```text
remote: A -> R
local:  A -> L
```

दोनों sides पर unique commits हैं। `--ff-only` safely stop करेगा क्योंकि simple pointer move से both
histories preserve नहीं हो सकतीं:

```text
fatal: Not possible to fast-forward, aborting.
```

Then fetch/history inspect करो, merge या rebase strategy consciously choose करो, conflicts solve,
tests run और review करो। Blind force/reset मत करो।

## Merge versus rebase pull

Possible strategies:

```powershell
git pull --no-rebase origin main
git pull --rebase origin main
git pull --ff-only origin main
```

- `--no-rebase`: fetched history merge करता है; divergence पर merge commit हो सकती है।
- `--rebase`: local commits fetched tip पर replay कर सकता है; local commit IDs बदल सकती हैं।
- `--ff-only`: केवल straight safe advance; divergence पर abort।

कोई one option हर team situation का universal answer नहीं। Team policy और shared-history status
matter करता है। Beginner safety के लिए current aligned main पर explicit `--ff-only` use हुआ।

## Plain `git pull` configuration-dependent hai

```powershell
git pull
```

Configured upstream और settings such as pull rebase/fast-forward behavior use कर सकता है। इसलिए
different machines पर divergent case behavior differ हो सकता है। Commands और repository/team config
understand करो; tutorial snippets blindly copy मत करो।

## `FETCH_HEAD` kya hai?

Fetch operation `.git/FETCH_HEAD` में fetched refs/commit information record कर सकती है। Easy meaning:

```text
FETCH_HEAD = अभी fetch से आया selected result का temporary Git reference/info
```

यह branch name या permanent remote repository नहीं। Normally manually edit नहीं करना।

## Pull se pehle clean state kyun?

Uncommitted changes हों और incoming changes same files touch करें, integration block/conflict या
confusing mixed state पैदा कर सकती है। Safe steps:

```text
status inspect
  -> current work commit / intentionally preserve
  -> correct branch and remote verify
  -> fetch/pull strategy choose
```

Stash exist करता है but magic backup नहीं; अभी उसका detailed use curriculum scope नहीं।

## Conflict easy example

Local और remote ने same line differently बदली:

```text
local:  PORT=3000
remote: PORT=4000
```

Git automatically सही business decision नहीं जानता। Conflict markers आ सकते हैं:

```text
start marker: <<<<<<< HEAD
PORT=3000
separator:    =======
PORT=4000
end marker:   >>>>>>> fetched-commit
```

Developer को intended value decide, markers हटाने, syntax/tests verify और integration complete करनी
होती है। दोनों lines blindly रखना हमेशा सही नहीं।

## Incoming code ko trust mat karo

Pull remote content local disk पर लाती है। उसके बाद:

- diff/log inspect करो;
- dependency manifest/lockfile changes देखो;
- install scripts और configuration changes review करो;
- tests चलाओ;
- secrets या suspicious code check करो।

Trusted repository भी compromised account/dependency से risk ला सकती है।

## Data/control flow

```text
verify clean state, branch, URL
  -> connect/authenticate to remote
  -> download missing objects and update remote-tracking info
  -> select fetched branch/FETCH_HEAD
  -> fast-forward, merge, rebase or abort
  -> update local branch/working tree if integration succeeds
```

Pull application deploy, database migrate या server process restart नहीं करता।

## Safe pull workflow

```text
git status --short --branch
  -> git remote -v
  -> confirm current branch/upstream
  -> preserve current work
  -> git fetch origin (often inspect-first workflow)
  -> git log --graph --oneline --decorate --all
  -> choose team-approved integration strategy
  -> git pull --ff-only origin main when straight advance expected
  -> inspect status/log/diff
  -> run tests
```

Current lesson direct pull command uses safe verified clean/aligned state; complex team work में
fetch-first review अक्सर बेहतर है।

## Common errors aur easy fixes

### `Not possible to fast-forward, aborting`

Local और remote history diverged हो सकती हैं। Good: command ने surprise merge रोकी। Fetch graph
inspect और deliberate merge/rebase plan बनाओ।

### Local changes would be overwritten

Uncommitted work incoming update से conflict करता है। Work delete मत करो। Status/diff inspect करके
commit या safely preserve करो, फिर pull retry करो।

### Merge conflict

Same/nearby content incompatible बदला। Conflict markers और both intentions समझो, correct result edit,
tests run और integration finish करो।

### Authentication failed

Remote read credential missing/expired या URL wrong हो सकती है। Secure auth setup और exact remote
verify करो; token command/URL में expose मत करो।

### `There is no tracking information for the current branch`

Plain pull को upstream नहीं मिला। Correct remote/branch explicitly specify या intentional upstream
configure करो; branch guess मत करो।

### Pull ke baad unexpected merge commit

Plain pull ने configured/default merge strategy use की और histories diverged थीं। Graph inspect करो;
published history rewrite करने से पहले team से coordinate करो। Future में explicit strategy use करो।

### `Already up to date` but website has newer content

Wrong remote/branch देख रहे हो सकते हो या UI branch अलग है। Remote URL, requested branch और live refs
verify करो।

## Student exercise

1. `git pull` के two main steps क्या हैं?
2. Fetch और pull में basic difference क्या है?
3. `--ff-only` divergence पर क्या करता है?
4. क्या pull local commits remote भेजता है?
5. Pull से पहले status क्यों देखते हैं?
6. Conflict में Git हमेशा सही value choose कर सकता है?

## Exercise answers

1. Fetch, then integration.
2. Fetch remote history लाता है; pull उसे current branch में integrate भी करता है।
3. Abort/stop करता है, surprise merge/rebase नहीं करता।
4. नहीं, वह push का काम है।
5. Uncommitted work overwrite/conflict risk पहचानने के लिए।
6. नहीं; developer को business intention decide करनी होती है।

## Interview question with Hinglish answer

**Question:** `git pull` क्या करता है और `--ff-only` क्यों useful है?

**Answer:** `git pull` पहले remote changes fetch करता है, फिर fetched branch को current local branch में
merge या rebase strategy से integrate करता है। `--ff-only` केवल straight fast-forward allow करता है
और divergence पर abort करता है, इसलिए unexpected merge commit से बचाता है। Pull से पहले मैं clean
status, branch, remote और team strategy verify करता हूँ।

## Easy-English minimum interview answer

> `git pull` fetches remote commits and then integrates them into the current local branch. The
> integration may use merge or rebase, depending on the command and configuration. `--ff-only`
> allows only a simple fast-forward and stops when the histories have diverged.

## Completion boundary

Topic 54 में clean/aligned `main` पर `git pull --ff-only origin main` successful no-op के रूप में
verify हुआ। HEAD और files unchanged रहे। No conflict, merge commit, rebase, force, reset, new commit,
push या application change हुआ। Topic 55 — Safe Git workflow अभी start नहीं हुआ।
