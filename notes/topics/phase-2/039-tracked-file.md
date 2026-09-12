# Topic 39 — Tracked file

## Learning goal

Tracked file ka exact Git meaning, tracking identity versus current change state,
HEAD/index/working-tree relationship, ignored-rule behavior and safe stop/delete workflows
samajhna hai. Staging area Topic 40 mein detail se aayegi.

## Simple definition

**Tracked file woh path/content hai jise Git ka index currently knows—because it was
previously recorded or intentionally added to the index. Git uske changes ko known baseline
ke against compare kar sakta hai.**

```text
Path known to Git index
  -> tracked
  -> working content baseline/index se compare ho sakta hai
```

Tracked ka meaning unchanged, committed right now, safe or correct nahi.

## Current real example

Path:

```text
notes/BACKEND_ROADMAP.md
```

Tracked query:

```powershell
git ls-files --error-unmatch "notes/BACKEND_ROADMAP.md"
```

Result:

```text
notes/BACKEND_ROADMAP.md
exit code: 0
```

Exit `0` and returned path prove current index knows it.

## Index entry evidence

```powershell
git ls-files --stage -- "notes/BACKEND_ROADMAP.md"
```

Verified:

```text
100644 8715ca9a0ea7439c227084e8e20dae7b1fd866ab 0 notes/BACKEND_ROADMAP.md
```

Beginner interpretation:

```text
100644    -> normal tracked file mode metadata
8715...   -> indexed content object identity
0         -> normal non-conflicted index stage slot
path      -> tracked repository-relative path
```

This hash index-selected content ka identity hai, current unsaved/editor state ka nahi.
Conflict stages advanced Git topic mein.

## HEAD evidence

```powershell
git ls-tree -r --name-only HEAD -- "notes/BACKEND_ROADMAP.md"
```

Path returned, so current `HEAD` commit snapshot also contains it. New file can index mein
tracked/staged ho before first commit, so “tracked” and “already in HEAD” universally same
nahi.

## One file ke multiple state dimensions

Current roadmap file:

```text
Tracked?             yes
Present in HEAD?     yes
Present in index?    yes
Working-tree changed? yes
Staged change?       no
Ignored?             tracking already active; ignore does not hide its change
```

Therefore:

```text
tracked + modified + unstaged
```

These labels describe different questions, not mutually exclusive categories.

## Tracked file states

A tracked path can be:

```text
Unmodified -> working content matches index
Modified   -> working content differs from index
Staged     -> index differs from HEAD
Deleted    -> tracked path absent from working tree
Renamed    -> old/new paths presented based on content/path changes
Conflicted -> index contains unresolved merge stages
```

Some combinations coexist:

```text
tracked + modified + unstaged
tracked + modified + staged
tracked + staged + modified again
tracked + deleted + unstaged/staged
```

## Three-state comparison

```text
HEAD snapshot
  ↕ compare with index
Index/staging selection
  ↕ compare with working tree
Working file on disk
```

Current example:

```text
HEAD roadmap content == index roadmap content
index roadmap content != working-tree roadmap content
```

Hence unstaged tracked modification.

## Tracked does not mean Git records every save

Git continuously every keystroke/version record nahi karta:

```text
tracked file saved on disk
  -> working tree changes
  -> Git detects difference
  -> history unchanged until deliberate stage + commit
```

Editor autosave and Git history separate.

## How untracked becomes tracked

Conceptual lifecycle:

```text
new file on disk
  -> untracked
  -> intentional add to index
  -> tracked + staged
  -> commit
  -> tracked in history
```

`git add` Topic 47 mein. Today current topic notes remain untracked.

## How tracked file changes

```text
known baseline/index content
  -> developer edits and saves
  -> Git compares bytes/content metadata
  -> modified tracked state
  -> review and stage later
```

Git JavaScript semantics nahi samajhta; textual/content difference track karta hai.

## Tracked file deletion

Tracked file filesystem se delete ho to Git can report deletion relative to index/HEAD.
Deletion history mein record tabhi after stage/commit. Accidental deletion before commit
may be recoverable from recorded version, but current uncommitted edits loss risk remain.

Delete/restore commands Topic 39 mein execute nahi hue.

## Stop tracking versus delete from disk

Two different intents:

```text
A. file repository and disk both se remove
B. file disk par retain but future Git tracking stop
```

Commands/options differ. Before action clarify which intent, whether file already contains
secret in history, and impact on teammates. `.gitignore` alone already tracked path ko
untrack nahi karti.

## Ignored rule and tracked file

Important:

> Ignore rules primarily untracked paths ko normal consideration se exclude karti hain;
> already tracked file changes normally remain visible.

If tracked `config.env` later `.gitignore` mein add karo, historical/tracked state magically
remove nahi. Secret already committed ho to rotation/history response required.

## Tracked versus untracked

| Question | Tracked | Untracked |
|---|---|---|
| Index knows path? | Yes | No |
| Known comparison baseline? | Yes | No Git tracked baseline |
| Normal `git diff` content? | Modified tracked content can show | Normally omitted until staged |
| Git recovery from commit? | Recorded versions may exist | Not guaranteed/no commit snapshot |
| Ignore rule hides it? | Not normally once tracked | Can if matching rule |

## Tracked versus committed

New file staged in index becomes tracked before first commit. Conversely committed file is
tracked in normal checkout unless removed from index. So:

```text
tracked = current index relationship
committed = repository history snapshot relationship
```

## Tracked versus staged

Every staged path is being tracked in index, but every tracked file does not have staged
changes. Current roadmap demonstrates:

```text
tracked: yes
staged change: no
working modification: yes
```

## File modes and cross-platform nuance

Index entry `100644` stores Git mode metadata, not complete Windows ACL permissions. Git
tracks limited mode/type information; filesystem permission systems separate. Linux
executable-bit behavior can matter even when Windows local behavior differs.

## Rename tracking nuance

Git snapshots paths/content; rename often deletion + addition similarity detection ke
through displayed. “Tracked identity permanently follows filename” oversimplification hai.
After rename, status/diff and staging review necessary.

## Tracked binary/generated files

Git technically many file types track kar sakta hai. Should track is policy question:

- source/docs/tests/lockfile commonly useful;
- large binaries/build outputs/logs/dependencies may bloat/noise;
- generated but critical artifacts policy-based;
- secret never safe just because private repository.

## History query

```powershell
git log -1 --oneline -- "notes/BACKEND_ROADMAP.md"
```

Current latest commit touching path:

```text
1f39efa docs: complete TaskForge phase 1 environment foundation
```

This proves recorded path history exists. It does not mean current working changes are in
that commit.

## Current evidence commands

```powershell
git ls-files --error-unmatch <path>
git ls-files --stage -- <path>
git ls-tree -r --name-only HEAD -- <path>
git diff --name-only -- <path>
git diff --cached --name-only -- <path>
git log -1 --oneline -- <path>
```

All read-only for this lesson.

## Interpretation flow

```text
Does index know path?
  -> no: untracked candidate
  -> yes: tracked
       -> compare working tree with index
       -> compare index with HEAD
       -> determine modified/staged/unchanged/deleted states
```

## Safety before changing tracked state

```text
1. repository root/CWD verify
2. status inspect
3. file responsibility/ownership inspect
4. working diff read
5. staged diff read
6. secret/generated/unrelated content check
7. intended track/untrack/delete action clarify
8. execute exact scoped command later
9. status/diffs reverify
```

## Common mistakes aur fixes

### Tracked means committed current content

Compare working/index/HEAD separately.

### Tracked means unchanged

Tracked file modified/deleted/staged ho sakti hai.

### Save means Git history updated

Save only disk state; stage/commit deliberate.

### `.gitignore` tracked file ko hide karegi

Existing index tracking remains. Untrack workflow and secret response separate.

### Every tracked file should be committed

Review responsibility; accidentally tracked secret/generated file needs safe remediation.

### Tracked file delete is always recoverable

Last committed content may recover, uncommitted edits may not. Recovery before deletion.

### Index hash current working file hash assume

If working file modified but unstaged, index still prior selected content identity.

### Path history means current working edit recorded

`git log -- path` old commits shows; current diff separately inspect.

## TaskForge connection

Future tracked set should intentionally include:

```text
TaskForge source
tests
README/documentation
safe configuration examples
package manifest and lockfile according to policy
```

Should not include:

```text
real secrets
node_modules
logs/caches
environment-specific private files
```

Exact `.gitignore` Topic 46 and secrets Topic 56.

## Student exercise

Using roadmap path, answer:

1. tracked proof command/result;
2. index entry four parts;
3. present in HEAD?
4. working modified?
5. staged?
6. combined state label;
7. tracked vs committed;
8. why `.gitignore` later is insufficient for committed secret?

## Exercise answer

```text
git ls-files --error-unmatch returns path, exit 0
mode=100644, object hash=8715..., stage=0, repository-relative path
HEAD contains path: yes
working modified: yes
staged: no
state: tracked + modified + unstaged
tracked means index knows; committed means history snapshot contains content
ignore does not remove existing tracking/history; secret needs rotation/remediation
```

## Interview question with Hinglish answer

**Question:** Git mein tracked file kya hoti hai, aur tracked/modified/staged mein difference
kya hai?

**Answer:** Tracked file ka path Git index ko known hota hai, so Git known baseline ke
against changes compare kar sakta hai. Modified means working-tree content index se differ
karta hai. Staged means index content `HEAD` snapshot se differ karta hai and next commit
ke liye selected hai. Ek file tracked + modified + unstaged ho sakti hai. `.gitignore`
already tracked file ko automatically untrack nahi karti.

## Easy-English minimum interview answer

**A tracked file is known to Git's index. It can still be unchanged, modified, staged, or
deleted. Modified compares the working tree with the index, while staged compares the index
with the last commit.**

Short version:

**Tracked means Git knows the file; it does not mean the current content is already
committed.**

## Completion boundary

Topic 39 mein tracked identity, state combinations, index/HEAD/history evidence, ignore and
safety boundaries complete hue. **Topic 40 — Staging area** next hai aur abhi start nahi
hua.

