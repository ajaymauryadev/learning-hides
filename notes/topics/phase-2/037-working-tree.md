# Topic 37 — Working tree

## Learning goal

Git working tree ko current filesystem state ke roop mein samajhna, usko repository,
`HEAD`, staging/index, CWD and VS Code workspace se distinguish karna, aur clean/dirty
state ko read-only evidence se inspect karna hai. Untracked/tracked files Topics 38–39.

## Simple definition

**Working tree repository ke checked-out, developer-visible files and directories ka
current filesystem state hai jise hum editor/terminal se read and modify karte hain.**

```text
Repository history ka selected state
  -> working tree mein files materialize
  -> developer edits/adds/deletes
  -> working tree recorded state se differ kar sakti hai
```

Working tree “Git history” khud nahi; history ke against current work area hai.

## Current verified working tree

```text
Git top-level: C:/Users/ajaym/Desktop/Practicle
Inside work tree: true
Bare repository: false
HEAD short identity: 1f39efa
```

Topic-start inspection:

```text
Modified tracked paths: 4
Staged paths: 0
Untracked Phase 2 note files: 3
Ignored paths: 0
```

This is intentionally **dirty working tree** because Topics 34–36 changes user ne abhi
commit nahi kiye. Existing work preserve hua; Topic 37 ne stage/restore/commit nahi kiya.

## Working tree boundary

Current non-bare repository:

```text
Practicle/                         <- working-tree top-level
  LEARNING_MEMORY.md               <- developer-visible file
  notes/                           <- developer-visible directory
  taskforge-backend/               <- empty child project folder
  .git/                            <- repository metadata, not normal working content
```

`.git/` repository metadata directory working tree content ke roop mein commit nahi hoti.

## Working tree versus working directory

```text
Working tree      -> entire checked-out filesystem tree for repository
Working directory/CWD -> shell ki current folder location
```

Example:

```text
Working-tree root: C:\Users\ajaym\Desktop\Practicle
CWD could be:      C:\Users\ajaym\Desktop\Practicle\notes
```

CWD working tree ke andar ek subdirectory ho sakti hai; both same concept nahi.

## Working tree versus VS Code workspace

VS Code workspace editor scope hai. It can open:

- complete Git working tree;
- only a subfolder;
- multiple repository roots;
- folder outside Git.

So Explorer root ko Git working-tree root assume nahi; `git rev-parse --show-toplevel`
verify karo.

## Working tree versus repository

```text
Working tree -> current editable files on disk
Repository   -> committed objects/history/references in Git metadata
```

File working tree mein change hone se committed history automatically change nahi hoti.

## Three-state mental model

```text
HEAD / last selected commit
  -> recorded baseline snapshot

Index / staging area
  -> next commit ke liye selected content

Working tree
  -> disk par current developer-visible content
```

Comparisons:

```text
working tree vs index -> unstaged differences
index vs HEAD         -> staged differences
working tree vs HEAD  -> total current content difference (conceptually)
```

Exact staging/diff commands Topics 40, 47–49 mein.

## Checkout/materialization mental model

When Git selects/checks out a commit/branch, its snapshot working tree mein materialize
ho sakta hai:

```text
commit snapshot
  -> files/directories written as working state
  -> developer works
```

Checkout/switch operations existing changes overwrite/conflict kar sakte hain. User work
inspect/preserve kiye bina branch/checkout commands nahi.

## Working-tree changes ke types

Conceptual categories:

```text
Modified -> existing tracked path ka content/mode changed
Added/new -> new filesystem file; tracking state separately matters
Deleted  -> recorded path working tree se absent
Renamed  -> path/content relationship Git diff heuristics se present ho sakta
Type changed -> file/symlink etc. kind changed where supported
```

Git status representation Topic 45 mein detail se parse hogi.

## Saved versus unsaved edits

Git disk/filesystem state inspect karta hai. VS Code unsaved editor buffer disk par write
nahi hui to Git working-tree comparison mein change appear na kare.

```text
Editor buffer changed
  -> not saved: Git may see old disk content
  -> saved: working-tree file changes
```

Test/diff se pehle save state verify.

## Clean working tree

Clean commonly means Git ko tracked working/index differences or reportable untracked
items nahi mil rahe according to command/config scope.

Clean does **not** prove:

- tests pass;
- code correct/security safe;
- ignored secrets absent;
- database clean;
- remote synchronized;
- editor has no unsaved buffer;
- build artifacts outside repo absent.

## Dirty working tree

Dirty means current working/index state recorded baseline se differ karti hai or untracked
items present hain. Dirty automatically bad nahi—it is normal while developing.

Risk arises when changes unknown, mixed, unreviewed or about to be overwritten.

```text
Good development dirty state:
known scoped feature + tests/docs, understood by developer

Risky dirty state:
unknown mixed files + secrets + generated output + destructive command planned
```

## Read-only inspection commands

```powershell
git status --short
git diff --name-only
git diff --cached --name-only
git ls-files --others --exclude-standard
```

Topic 37 uses output classification only. Exact status columns/diffs/staging later topics.

## Current evidence breakdown

`git diff --name-only` returned tracked working-tree differences:

```text
LEARNING_MEMORY.md
notes/ARCHITECTURE.md
notes/BACKEND_ROADMAP.md
notes/LEARNING_STATE.md
```

`git diff --cached --name-only` returned no paths: staging selection empty at inspection.

`git ls-files --others --exclude-standard` returned Topic 34–36 notes. Topic 37 note is
created after baseline, so final untracked set naturally also includes it.

## Empty directory nuance

`taskforge-backend/` exists on filesystem and therefore working tree area mein directory
hai, but it contains no files. Git normally empty directory track/report nahi karta.

```text
Filesystem working area contains empty folder
Git snapshot/status has no file entry for it
```

Working tree concept is filesystem-oriented, while Git tracking records file content/tree
structure based on tracked entries.

## Ignored items nuance

Ignored file working directory/tree on disk mein exist kar sakti hai but normal status
omit kare. Therefore “clean” output all disk files absent/known prove nahi. `-Force` shell
listing and explicit Git ignored-file inspection different evidence.

Current ignored-path count check `0` returned for repository at inspection, but global
ignore permission warning context remains documented; conclusion ko that warning ke saath
read karo.

## Line endings and working-tree representation

Git stored/index form and Windows working-tree form line-ending rules ke through differ
kar sakte hain. Current `git diff` checks LF-to-CRLF working-copy warnings show karte hain.
Working-tree conversion warning necessarily content bug nahi; project policy and actual
diff inspect karna chahiye.

## File permissions/mode

Git some file mode metadata track kar sakta hai, but OS/filesystem support differs.
Windows/Linux behavior identical assume nahi. Executable-bit or permission issue mein
Git diff metadata and deployment platform inspect.

## Multiple working trees

Git linked worktrees feature one repository history ke saath multiple checked-out working
directories support kar sakti hai. Current setup simple single working tree hai. Feature
only parallel-work need justify kare tab.

## Running process and working tree

Running Node process source startup time par load kar chuka ho sakta hai. Working-tree file
edit se running memory automatically update only watcher/restart behavior par depend.

```text
working file saved
  != running process automatically refreshed
```

## Safe workflow with dirty tree

```text
1. Git top-level/CWD verify
2. status inspect
3. identify which changes belong to whom/topic
4. save unsaved intended edits
5. run relevant verification
6. inspect unstaged/staged differences separately
7. preserve unrelated work
8. only intended content stage/commit later
```

## Destructive-operation warning

Commands that restore, reset, clean, checkout or remove can overwrite/delete working-tree
work. Exact command semantics and target scope samjhe bina run nahi.

Never assume “uncommitted means disposable.” Current Topic 34–36 changes valuable user
work hain.

## Common mistakes aur fixes

### Working tree = current folder

Use top-level vs CWD distinction; current folder may be nested subdirectory.

### Save means committed

Save working-tree disk state update; commit separate repository record.

### Clean means application healthy

Run tests/security/config/service checks separately.

### Dirty means error

Understand changes; development naturally creates differences.

### Empty folder missing because Git status silent

Filesystem `Test-Path` separately inspect; Git tracks no standalone empty directory.

### Unsaved code missing from diff

Editor buffer save state inspect.

### Delete/restore unknown changes

Ownership and intent establish; diff/backups/recovery before mutation.

### Child project status treated as isolated

Child inherits parent root currently, so Git scope may include learning notes outside child.

## TaskForge connection

Future working tree:

```text
Practicle/ repository working tree
  notes/                         -> curriculum docs
  taskforge-backend/             -> project files later
    package.json
    src/
    tests/
```

Backend edit/test/debug occurs working tree mein; verified reviewed snapshot later commit
history mein. Repository boundary decision remains pending ordered topics.

## Student exercise

1. Working tree one sentence define.
2. Working tree vs CWD.
3. Working tree vs repository.
4. HEAD/index/working tree diagram draw.
5. Clean state kya prove nahi karti—four examples.
6. Current modified/staged/untracked counts explain.
7. Unsaved editor buffer Git diff mein kyun absent ho sakta hai?
8. Unknown dirty work preserve kyun karna hai?

## Exercise answer

```text
working tree = current checked-out developer-visible filesystem state
CWD = shell's current folder, possibly inside working tree
repository = Git metadata/history
HEAD = recorded baseline; index = selected next snapshot; working tree = disk state
clean does not prove tests/security/remote sync/no ignored secrets
baseline: 4 modified tracked, 0 staged, 3 untracked topic notes
unsaved buffer disk par nahi, Git disk state compares
unknown dirty work another developer/user ka valuable work ho sakta hai
```

## Interview question with Hinglish answer

**Question:** Git working tree kya hoti hai aur clean/dirty ka kya meaning hai?

**Answer:** Working tree repository ka current checked-out developer-visible filesystem
state hai jahan files edit/add/delete hoti hain. It is different from repository history,
staging index and shell CWD. Clean means Git ko current scope mein reportable differences
nahi mil rahi; dirty means working/index state recorded baseline se differ karti hai or
untracked items hain. Clean application correctness or ignored-secret absence prove nahi.

## Easy-English minimum interview answer

**The working tree is the current checked-out set of files on disk that a developer edits.
A clean working tree has no reportable Git changes, while a dirty working tree has modified,
staged, deleted, or untracked work. Clean does not mean the application is correct.**

Short version:

**The working tree is the editable project state on disk, compared by Git with the staged
and committed states.**

## Completion boundary

Topic 37 mein working-tree definition, boundaries, three-state comparison, clean/dirty
meaning, evidence and safety complete hue. **Topic 38 — Untracked file** next hai aur abhi
start nahi hua.

