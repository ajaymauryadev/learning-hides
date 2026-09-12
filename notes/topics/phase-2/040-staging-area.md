# Topic 40 — Staging area

## Learning goal

Git staging area/index ko next commit ke proposed snapshot ke roop mein samajhna, “nothing
staged” versus “empty index,” working/index/HEAD comparisons, partial staging and safe
review discipline learn karna hai. `git add` Topic 47 and staged diff Topic 49.

## Simple definition

**Staging area—Git index—next commit ke liye currently selected file contents and path/mode
entries ka proposed snapshot hota hai.**

```text
Working tree -> select content -> Index/staging area -> commit later -> Repository history
```

It lets developer decide current disk changes mein se next commit mein exactly kya jaana
chahiye.

## Staging area folder nahi

Staging area koi visible `staging/` project folder nahi. It is Git-managed index data,
commonly repository metadata under `.git/index`.

```text
Practicle/
  source/docs files       -> working tree
  .git/index              -> Git-managed index representation
```

`.git/index` manually edit/delete nahi. Git commands through manage.

## Three-state model

```text
HEAD
  = last selected committed snapshot

Index / staging area
  = next commit candidate snapshot

Working tree
  = current files on disk
```

Comparisons:

```text
Working tree vs index -> unstaged changes
Index vs HEAD         -> staged changes
```

Commit normally index snapshot record karta hai, every current working-tree byte nahi.

## Current real evidence

```powershell
git diff --cached --name-only
git diff --cached --name-status
```

Both returned zero paths:

```text
StagedNameCount=0
StagedStatusCount=0
```

Meaning: index currently `HEAD` se reportably differ nahi karta.

## “Nothing staged” does not mean empty index

```powershell
git ls-files --stage
```

Current index entry count:

```text
48
```

So:

```text
Index contains tracked snapshot entries: 48
Index versus HEAD differences: 0
```

Staging area tracked project snapshot maintain karti hai. “Empty staging area” everyday
phrase usually **no staged differences** means, literally zero entries nahi.

## Current index entry example

```text
100644 8715ca9a0ea7439c227084e8e20dae7b1fd866ab 0 notes/BACKEND_ROADMAP.md
```

```text
100644  -> Git file mode
8715... -> index-selected content object identity
0       -> normal non-conflicted index stage
path    -> repository-relative path
```

Working roadmap has new edits, but index still older selected content because it has not
been staged.

## Current state comparison

```text
Working-tree differences: 4 tracked files
Staged differences: 0
Untracked Phase 2 notes: present
Index tracked entries: 48
```

Conceptual:

```text
HEAD == index
index != working tree for four tracked files
untracked files have no normal index entries yet
```

## Staging copies/selects content; file move nahi hoti

When content staged later:

```text
working file remains on disk
selected content snapshot goes into index
```

File project folder se `.git` folder mein physically “move away” nahi hoti. Index records
selected content/path state for commit.

## Staging is not committing

```text
Stage -> prepare proposed snapshot locally
Commit -> proposed index snapshot ko repository history record
Push -> local commits remote repository ko send
```

Three separate operations/evidence. Staged content remote par nahi and durable shared
history record nahi.

## Why staging area exists

Suppose same working session:

```text
auth bug fix
README typo
temporary debug log
unfinished analytics experiment
```

Staging area enables only coherent auth fix + matching test/docs select karna. Without
review, broad selection unrelated work mix kar sakti hai.

## Atomic commit preparation

```text
Working tree contains many changes
  -> inspect all
  -> stage one coherent purpose
  -> staged diff review
  -> commit that purpose
  -> remaining changes stay working tree
```

Staging quality commit quality improve karti hai, but developer judgment still needed.

## Whole-file staging

Conceptually file ka current saved content index snapshot mein select hota hai:

```text
working version W1 -> stage -> index version W1
```

Exact `git add <path>` Topic 47.

## Stage then edit again

Important state:

```text
working W1 -> stage -> index W1
then working file edit -> working W2

HEAD: old H
Index: W1          -> staged difference
Working: W2        -> additional unstaged difference
```

Same file simultaneously staged and unstaged modifications rakh sakti hai. Commit W1
record karega, W2 automatically nahi—unless restaged.

## Partial staging

One file ke selected hunks/lines stage karna possible:

```text
file changes:
  hunk A -> feature fix, stage
  hunk B -> unrelated refactor, leave unstaged
```

Useful but beginner risk: selected code incomplete/invalid ho sakta hai because working
tree tests W1+W2 combination run karte hain while commit candidate only W1. Staged snapshot
independently inspect/test strategy required.

## New file staging

Untracked file selected into index:

```text
untracked -> tracked + staged addition
```

Working file remains disk par. Commit later history record.

## Deletion staging

Tracked file working tree se removed, then deletion selected:

```text
HEAD/index initially contain path
working tree path absent -> unstaged deletion
stage deletion -> index path removed relative to HEAD
commit -> history records deletion in new snapshot
```

Prior commits may still contain old content, including secrets.

## Rename/mode staging

Staging can select new path/deleted old path/mode change. Git may present rename based on
similarity. Always staged diff inspect; label alone business intent prove nahi.

## Unstaging concept

Unstage means index selection ko HEAD-like state toward restore while normally preserving
working file edits. Exact commands/options later safe Git workflow mein.

```text
Index selection changed
Working file should remain—verify command semantics before use
```

Unstage is not same as discard working changes.

## Index and merge conflicts

During conflict, index can hold multiple stage entries (base/ours/theirs-like data). Normal
entry stage number `0`; unresolved states other stage slots use kar sakte hain. Advanced
merge topic later. Manually index edit nahi.

## Staging and ignored files

Ignored file normal add selection se excluded/resisted ho sakti hai, but force options can
override. Force staging secret/generated file dangerous. Ignore rule security boundary
nahi; staged snapshot always review.

## Staging and secrets

Secret risk point:

```text
secret file untracked
  -> broad add accidentally stages
  -> staged diff not reviewed
  -> commit/push exposure
```

Prevention:

- ignore before broad staging;
- exact paths prefer;
- staged name/status/content review;
- secret scanners later as additional control;
- exposure after commit requires rotation/remediation.

## Staging and generated files

`node_modules`, logs, coverage/build outputs can create huge staged sets. Before staging:

```text
responsibility -> ignore policy -> exact status -> selected add -> staged diff
```

Lockfile is tool-managed but often intentionally tracked; generated automatically does not
always mean ignored.

## Index is local state

Staging area current local repository/worktree state hai. It is not shared by push and not
normally part of commit history as an independent step. Another clone does not receive
your “currently staged but uncommitted” selection.

## Index can differ from both HEAD and working tree

```text
HEAD content:    A
Index content:   B
Working content: C
```

Therefore always inspect both:

```text
unstaged diff = C vs B
staged diff   = B vs A
```

Only `git status` summary insufficient for exact code review.

## Read-only inspection commands

```powershell
git diff --cached --name-only
git diff --cached --name-status
git ls-files --stage
git diff --name-only
git status --short
```

Topic 40 used only inspections. `git add`, restore/reset and commit not run.

## Safe staging workflow preview

```text
1. repository root/CWD verify
2. working status and untracked paths inspect
3. working diffs/content review
4. secrets/generated/unrelated work classify
5. exact intended paths/hunks stage
6. staged names/status inspect
7. staged content diff inspect
8. verify proposed snapshot coherently
9. commit only after review
10. recheck remaining working tree
```

Commands Topics 45–50 mein one-by-one.

## Common mistakes aur fixes

### Staging area is empty because cached diff empty

No staged differences means index matches HEAD; index still tracked entries contains.

### Stage means commit

Index selection local and mutable; commit separate history record.

### Stage moves file out of working tree

It selects/copies content state; disk file remains.

### Save after staging updates staged content automatically

No. Later edit creates unstaged difference until restaged.

### `git add .` creates good commit automatically

Broad scope may include unrelated/secrets/generated files. Review/select.

### Only working-tree tests guarantee staged snapshot

Partial/staged-old version may differ. Staged candidate independently reason/verify.

### Unstage discards work

Conceptually unstage changes index selection, discard changes working content; exact command
semantics verify.

### Ignore rule guarantees secret cannot stage

Force/options/already tracked files bypass assumption. Staged review mandatory.

## TaskForge connection

Future feature work:

```text
working tree:
  source implementation
  matching tests
  API/docs update
  unrelated experiments

staging area for one commit:
  only coherent feature implementation + tests/docs
```

This supports reviewable professional history.

## Student exercise

1. Staging area one sentence define.
2. Why cached diff count 0 but index entry count 48?
3. HEAD/index/working tree draw.
4. Stage then edit scenario H/W1/W2 explain.
5. Stage vs commit vs push.
6. Partial staging benefit/risk.
7. Secret prevention before staging.
8. Why unstage and discard different?

## Exercise answer

```text
staging = next commit proposed snapshot/index
0 cached differences means index matches HEAD; it still holds 48 tracked entries
HEAD baseline -> index candidate -> working disk state
after stage W1 then edit W2: index W1, working W2
stage prepares; commit records locally; push shares commits
partial stage isolates purpose but candidate may differ from tested working combination
inspect/ignore secrets and review staged diff
unstage changes index; discard changes working file content
```

## Interview question with Hinglish answer

**Question:** Git staging area kya hai aur working tree/commit se kaise different hai?

**Answer:** Staging area ya index next commit ka proposed snapshot hai. Working tree disk
par current editable files hain; index selected versions रखता hai; commit index snapshot ko
local history mein record karta hai. File stage karne ke baad edit karun to same file staged
and unstaged changes dono rakh sakti hai. Isliye working diff and staged diff separately
review karta hoon.

## Easy-English minimum interview answer

**The staging area, also called the index, stores the proposed snapshot for the next commit.
The working tree contains current files on disk, and a commit records the staged snapshot
in local history.**

Short version:

**Staging selects exactly what the next commit should contain; it does not create the
commit itself.**

## Completion boundary

Topic 40 mein index purpose, non-empty-index nuance, three-state comparisons, partial/new/
deleted staging and safety complete hue. **Topic 41 — Commit** next hai aur abhi start nahi
hua.

