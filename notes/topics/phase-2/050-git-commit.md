# Topic 50 — `git commit`

## Learning goal

Simple examples se samajhna hai ki commit kya record karta hai, staging area ka role kya hai,
commit message kyun important hai aur commit push/deployment se kaise अलग है। Reviewed Topics 48–50
documentation ko one meaningful local commit mein record karna hai.

## Sabse simple definition

**`git commit` staging area ke current snapshot ko local repository history mein permanently
addressable record ke roop mein save karta hai.**

```text
working files -> git add -> staging area -> git commit -> local history
```

Commit केवल staged snapshot लेता है। Unstaged और untracked content automatically include नहीं होता।

## Save-game wala easy example

Imagine game khel rahe ho:

```text
current gameplay        = working tree
save ke liye selection  = staging area
named save point        = commit
cloud synchronization   = push
```

Commit local “save point” जैसा है, लेकिन Git project files का snapshot और history relationship
record करता है। Commit होने का मतलब GitHub पर upload होना नहीं है।

## Practical command

```powershell
git commit -m "docs: complete phase 2 topics 48 to 50"
```

Command anatomy:

```text
git       -> Git program
commit    -> staged snapshot history mein record karo
-m        -> message command line par do
"..."     -> short meaningful commit message
```

## Commit se pehle exact preparation

Topic start par only Topic 48 lesson staged thi. Topic 49, Topic 50 aur trackers ko पहले exact paths
से select करना जरूरी था:

```powershell
git add -- LEARNING_MEMORY.md
git add -- notes/ARCHITECTURE.md
git add -- notes/BACKEND_ROADMAP.md
git add -- notes/LEARNING_STATE.md
git add -- notes/topics/phase-2/048-git-diff.md
git add -- notes/topics/phase-2/049-git-diff-cached.md
git add -- notes/topics/phase-2/050-git-commit.md
```

Then review:

```powershell
git status --short
git diff --cached --name-status
git diff --cached --check
```

Only expected seven paths staged होने पर commit allowed था।

## Commit ke andar kya hota hai?

Simplified commit object:

```text
commit
|-- project snapshot ka tree reference
|-- parent commit reference
|-- author identity + author time
|-- committer identity + commit time
`-- commit message
```

Commit changed lines की loose list मात्र नहीं; complete project snapshot ko Git objects/references
से represent करता है। Storage efficient हो सकती है, लेकिन mental model snapshot है।

## Commit ID / hash

Har commit ko hash मिलता है, example:

```text
a5a3b514a15cc10dd81192baef09ad4a8a63e57f
```

Short form:

```text
a5a3b51
```

Hash commit content और metadata से derived identity है। History rewrite/amend होने पर ID बदल सकती
है। Short hash convenient है, full hash exact identity है।

## Parent relationship

Normal new commit previous current commit को parent मानता है:

```text
older commit <- parent -- new commit <- main/HEAD
```

Commit success के बाद current branch reference और `HEAD` new commit पर move करते हैं। Old commit
history में parent के रूप में रहता है।

## Staged, unstaged aur untracked example

Before commit:

```text
M  staged-file.md
 M unstaged-file.md
?? new-untracked.md
```

Normal commit में:

```text
staged-file.md     -> included
unstaged-file.md   -> latest unstaged content not included
new-untracked.md   -> not included
```

यही कारण है कि commit से पहले cached diff सबसे important review है।

## Good commit message

Message future developer को intent बताना चाहिए:

```text
Good: docs: explain cached diff and commit workflow
Weak: update
Weak: changes
Weak: final final
```

Simple pattern:

```text
type/scope hint: imperative or outcome-focused summary
```

Examples:

```text
feat: add task creation endpoint
fix: reject expired workspace invitation
test: cover unauthorized task update
docs: explain Git staging workflow
```

Convention team-specific हो सकती है; clarity mandatory है, exact prefix universal law नहीं।

## One commit, one coherent idea

Good commit review/revert आसान बनाता है:

```text
one logical learning update -> one commit
```

Unrelated feature, formatting और secret/config change ko one विशाल commit में mix नहीं करना चाहिए।
Current practical commit Topics 48–50 Git review/recording learning unit है।

## Commit is local

```text
git commit -> local repository history changes
git push   -> local commits remote par publish (Topic 53)
deployment -> application environment update, separate process
```

Commit success internet connection prove नहीं करता।

## Commit ke baad expected state

If all working changes staged and committed:

```text
HEAD/new branch tip -> new commit
index                -> new HEAD snapshot
working tree         -> may be clean
```

Working tree तभी clean होगी जब commit के बाहर कोई later/untracked/unstaged change न बचे। Commit
success alone clean status guarantee नहीं करता।

## Author and committer

Git commit के लिए identity config चाहिए:

```powershell
git config user.name
git config user.email
```

- Author: original change author.
- Committer: commit record बनाने वाला.

Normal local workflow में दोनों same हो सकते हैं। Email Git history metadata बन सकती है, इसलिए
intentional identity/privacy choice करो। Current repository में identity configured मिली।

## `git commit -a` caution

```powershell
git commit -am "message"
```

`-a` tracked modified/deleted paths को automatically stage कर सकता है, लेकिन new untracked files
include नहीं करता। यह explicit review bypass करा सकता है; beginner safe workflow में separate
`git add`, cached diff review, then commit बेहतर है।

## `--amend` caution

```powershell
git commit --amend
```

Last commit को edit-in-place नहीं करता; replacement commit बनाता है और hash बदलता है। Published
shared history पर amend collaboration problems पैदा कर सकता है। Current lesson में amend नहीं हुआ।

## Hooks and commit failure

Repository Git hooks commit से पहले checks चला सकते हैं। Hook fail होने पर commit create नहीं हो
सकता। Error output पढ़कर actual issue fix करो; hooks bypass करना default solution नहीं।

## Data/control flow

```text
Git reads index snapshot
  -> creates tree/commit objects
  -> records parent + identity + time + message
  -> moves current branch reference
  -> HEAD resolves to new commit
```

Working files normally rewrite नहीं होतीं और remote request नहीं जाती।

## Safe commit workflow

```text
git status --short
  -> git diff
  -> exact git add
  -> git diff --cached --name-status
  -> git diff --cached
  -> git diff --cached --check
  -> git commit -m "meaningful message"
  -> git status
  -> git log -1 (Topic 51 detail later)
```

## Common errors aur easy fixes

### `nothing to commit`

Index में HEAD से अलग staged snapshot नहीं है। Status check करो; शायद changes unstaged/untracked हैं
या already committed हैं। Empty commit blindly मत बनाओ।

### `Author identity unknown`

Git को name/email config नहीं मिली। Correct scope (repository या global) और privacy-aware identity
set करनी होगी। Random identity मत डालो।

### Wrong files commit हो गईं

Cached diff review skip हुआ। अगर commit publish नहीं हुआ, safe correction options समझकर choose करो;
blind reset/amend मत चलाओ। Published commit के लिए team-safe new correction commit often बेहतर है।

### Commit हुआ लेकिन GitHub पर नहीं दिखा

Expected: commit local है। Remote publication के लिए push separate Topic 53 operation है।

### Latest file edit commit में नहीं आई

Edit staging के बाद हुई थी। Commit ने index version लिया। Remaining change status/plain diff में
देखो और next coherent commit में add करो।

### Commit message editor खुल गया

`-m` नहीं दिया गया, इसलिए Git configured editor खोल सकता है। Message लिखकर save/close करना पड़ता
है; empty message commit abort कर सकती है।

### Hook ने commit रोक दिया

Hook output/test/lint error पढ़ो और root cause fix करो। `--no-verify` से bypass करना normal answer
नहीं है।

## Student exercise

1. Commit कौनसा content लेता है?
2. क्या untracked file automatically commit होती है?
3. Commit और push में क्या difference है?
4. Commit के बाद branch कहाँ point करती है?
5. Meaningful message क्यों चाहिए?
6. `--amend` का मुख्य risk क्या है?

## Exercise answers

1. Staging area/index का current snapshot.
2. नहीं, पहले intentionally stage करनी होती है।
3. Commit local history record; push commits remote पर publish करता है।
4. Newly created commit पर।
5. Future readers को change का intent जल्दी समझ आता है।
6. Replacement commit बनाकर hash/history बदलता है, especially published history risky है।

## Interview question with Hinglish answer

**Question:** `git commit` क्या करता है?

**Answer:** `git commit` staging area के current snapshot को local Git history में record करता है।
Commit में project snapshot reference, parent, author/committer metadata और message होता है। यह केवल
staged content लेता है और automatically remote पर push नहीं करता।

## Easy-English minimum interview answer

> `git commit` records the staged snapshot in the local repository history. A commit contains a
> project snapshot reference, a parent, identity metadata, and a message. It does not automatically
> include untracked changes or push anything to a remote repository.

## Completion boundary

Topic 50 में reviewed Topics 48–50 documentation one local commit में record हुई। Commit identity,
parent, message, staged-only boundary और post-commit state verify हुए। कोई push, pull, amend, reset या
application change नहीं हुआ। Topic 51 — `git log` अभी start नहीं हुआ।
