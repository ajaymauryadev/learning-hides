# Topic 36 — Repository kya hai?

## Learning goal

Git repository ko ordinary folder/project se distinguish karna, repository boundary,
`.git` metadata, work-tree relationship, bare/non-bare forms, repository discovery and
current TaskForge layout samajhna hai. Working tree Topic 37 mein detail se aayega.

## Simple definition

**Git repository woh version-controlled data structure hai jisme project history,
snapshots/objects, references and Git metadata stored/manage hote hain; non-bare repository
ke saath usually developer-visible working files bhi hote hain.**

Beginner view:

```text
Repository context
  ├─ working files/directories
  └─ Git metadata/history (.git)
```

Sirf folder ka naam `repo` rakhne se repository nahi banti. Git metadata/discovery se
repository identity establish hoti hai.

## Folder, project aur repository

```text
Folder     -> filesystem container
Project    -> application/work ka logical boundary
Repository -> version-control history/metadata boundary
```

They can align:

```text
taskforge-backend/  <- folder + project root + Git repository top-level
  .git/
  src/
```

Or differ:

```text
company-platform/   <- Git repository root
  backend/           <- project folder
  frontend/          <- project folder
```

Current structure second pattern ke closer hai: `Practicle` Git root and
`taskforge-backend` child project folder.

## Repository top-level

Top-level repository work-tree boundary inspect:

```powershell
git rev-parse --show-toplevel
```

Current result:

```text
C:/Users/ajaym/Desktop/Practicle
```

This means Git commands run inside descendants commonly this repository context use
karenge, unless nested/separate Git boundary intentionally exists.

## `.git` directory

Normal non-bare repository mein `.git/` metadata directory commonly contains/manages:

```text
objects       -> content/history objects
refs          -> branch/tag-like references
HEAD          -> currently selected reference context
index         -> staging-area data
config        -> repository-local Git configuration
logs          -> reference movement logs where enabled
```

Exact internal files can vary. Manually edit/delete nahi; Git commands through state
manage karna safer hai.

Current command:

```powershell
git rev-parse --git-dir
```

At repository root result:

```text
.git
```

Relative output current location ke context mein hai. Child folder se same metadata ka
absolute path report hua:

```text
C:/Users/ajaym/Desktop/Practicle/.git
```

Output form relative/absolute differ hone se repository different prove nahi.

## Common Git directory

```powershell
git rev-parse --git-common-dir
```

Current result `.git`. Advanced linked worktree arrangements mein per-worktree metadata
and common shared metadata paths differ kar sakte hain. Abhi current repo simple layout
use karta hai; linked worktrees later only real need par.

## Repository discovery

Git command current directory se repository metadata find karta hai, commonly parent
directories upward search karke:

```text
CWD: Practicle/taskforge-backend
  -> .git here? no
  -> parent Practicle/.git? yes
  -> repository root = Practicle
```

This is why empty child inside repository still parent Git context inherit karta hai.

## Prefix inside repository

```powershell
git rev-parse --show-prefix
```

At root output empty tha, because root relative prefix none. Inside child:

```text
taskforge-backend/
```

Prefix means current folder repository top-level ke andar kis relative location par hai.
It does not mean child independent repository.

## Inside work tree versus inside Git directory

```powershell
git rev-parse --is-inside-work-tree
git rev-parse --is-inside-git-dir
```

Current root results:

```text
inside work tree = true
inside Git dir   = false
```

Meaning current CWD developer working-file area mein hai, `.git` metadata directory ke
inside nahi.

## Non-bare repository

```powershell
git rev-parse --is-bare-repository
```

Current result:

```text
false
```

Non-bare repository commonly developer working tree plus Git metadata provide karti hai:

```text
Practicle/
  .git/
  notes/
  taskforge-backend/
```

Local development/editing ke liye normal form.

## Bare repository

Bare repository mein normal checked-out working tree nahi hoti; Git metadata repository
directory itself hoti hai. Commonly central/shared remote endpoint role mein use hoti hai.

```text
project.git/
  objects/
  refs/
  HEAD
```

Bare ka meaning empty nahi. It can contain full history without developer working files.
Current repository bare nahi and Topic 36 ne bare repo create nahi ki.

## Local repository versus remote repository

```text
Local repository  -> current machine/process-accessible repo and history
Remote repository -> another repository referenced by a remote name/URL
```

“Remote” cloud-only necessarily nahi; another filesystem/server repository bhi ho sakti
hai. Detailed Topic 43/52.

## Clone versus init overview

```text
git init  -> existing/new folder mein repository metadata initialize
git clone -> another repository se history + working context create/copy
```

Commands later Topics. Current parent already repo hai, so child `git init` nested boundary
bana sakta hai. No mutation now.

## Repository boundary kya control karti hai?

Boundary impacts:

- `git status` ka file scope;
- ignore rules hierarchy;
- local config;
- branches/commits/history;
- hooks;
- remote mappings;
- CI trigger/repository permissions;
- review and ownership scope.

Wrong boundary unrelated projects/secrets include kar sakti hai or required files exclude.

## One repository, multiple projects

Single repository multiple related projects contain kare to often monorepo-style structure
kehlata hai:

```text
platform/
  backend/
  frontend/
  shared/
```

Benefits: coordinated changes and shared history. Costs: broader tooling/permissions/build
scope. Current `Practicle` has learning docs + TaskForge folder; final boundary deliberate
decision Phase 2 commands se pehle hogi, label blindly apply nahi.

## Multiple repositories

Separate repositories isolation, permissions/releases/history independently manage kar
sakti hain, but cross-repo coordinated changes harder. “Advanced” hone ke liye split nahi;
ownership/deployment/security needs justify karein.

## Nested repository

```text
parent/.git
parent/child/.git
```

Nested repo technically possible, but parent commonly child contents ko ordinary tracked
files ki tarah handle nahi karta once nested metadata/repository boundary involved; special
handling/submodule-like intent may be needed. Accidental nesting confusing status and
commits cause kar sakti hai.

Rule: `git init` se pehle `git rev-parse --show-toplevel` and `.git` existence inspect.

## Repository is not a server

Repository disk/history structure hai. It does not run TaskForge HTTP process:

```text
Git repository exists
  != Node server running
  != port listening
  != database connected
  != API healthy
```

## Repository is not production data

Source/test/docs history repository mein. User accounts/tasks MongoDB mein. Attachments,
secrets and logs require appropriate storage/policies. Git database ko application database
mat samjho.

## Repository identity and remotes

Local repository remote ke bina valid ho sakti hai. Remote mapping absent/present repository
identity ka only criterion nahi. A repository can have zero, one or multiple remotes.

## Repository health evidence layers

```text
metadata discoverable
  -> object/reference integrity
  -> working/index status readable
  -> expected history/branch
  -> configuration/remotes
  -> remote connectivity separately
```

`rev-parse` success basic discovery proves, all objects/remotes healthy nahi.

## Current verified repository record

```text
Top-level: C:/Users/ajaym/Desktop/Practicle
Git directory at root: .git
Common Git directory: .git
Inside work tree: true
Inside Git directory: false
Bare repository: false
Current branch: main
```

From `taskforge-backend/`:

```text
Top-level: C:/Users/ajaym/Desktop/Practicle
Git directory: C:/Users/ajaym/Desktop/Practicle/.git
Prefix: taskforge-backend/
Own .git: false
```

## Read-only execution flow

```text
CWD inside project child
  -> Git searches upward
  -> parent .git discovered
  -> repository top-level/metadata reported
  -> no state changed
```

## Common mistakes aur fixes

### Folder = repository assume karna

Use `git rev-parse --show-toplevel`; metadata/discovery verify.

### Project root = Git root assume karna

Compare project path with repository top-level.

### `.git` visible nahi, so repo absent

It may be hidden or in parent. `-Force` and `rev-parse` inspect.

### Child mein blindly `git init`

Parent discovery first; intended isolation and nested behavior decide.

### Bare repository empty samajhna

Bare lacks normal working tree but can contain history.

### `.git` folder copy = safe backup

Live/incomplete copy consistency issues possible. Git remote/bundle/backup strategy use
with verification.

### `.git` manually edit/delete

Metadata/history corruption/loss risk. Exact Git operation and recovery plan use.

### Remote = local repository

They are independent histories that may diverge until synchronization.

## Safe repository inspection checklist

```text
1. Get-Location
2. git rev-parse --show-toplevel
3. git rev-parse --git-dir
4. git rev-parse --is-inside-work-tree
5. git rev-parse --is-bare-repository
6. current prefix from intended folder
7. parent/nested .git boundaries inspect
8. git status read-only
9. no init/config/delete before boundary decision
```

## TaskForge repository decision questions

Before Topic 44:

1. Learning notes and backend same lifecycle/history need karte hain?
2. GitHub remote should contain whole curriculum repository or backend only?
3. CI/deployment root kya expect karega?
4. Secrets/permissions boundary kya honi chahiye?
5. Existing committed history preserve kaise hogi?
6. Nested repo/submodule complexity justified hai?

Decision evidence-based hoga. Topic 36 only model establishes.

## Student exercise

1. Folder/project/repository definitions.
2. Current top-level and `.git` path.
3. Bare vs non-bare difference.
4. Child prefix and own `.git` state.
5. Git parent search flow draw.
6. Repository existence server health kyun prove nahi.
7. Nested initialization se pehle checks.

## Exercise answer

```text
folder = filesystem container
project = logical application/work boundary
repository = Git history/metadata boundary
top-level = C:/Users/ajaym/Desktop/Practicle
git-dir = .git at root
non-bare = working tree + metadata; bare = no normal checked-out working tree
child prefix = taskforge-backend/; own .git = false
Git searches upward and finds parent .git
repository is storage/history, not running API
check CWD, top-level, git-dir and intended boundary before init
```

## Interview question with Hinglish answer

**Question:** Git repository kya hoti hai aur project folder se kaise different hai?

**Answer:** Git repository version-controlled history, objects, references and metadata ka
boundary hai; non-bare repository ke saath working files bhi hote hain. Project folder
logical application boundary hai and Git root se same hona required nahi. Main
`git rev-parse --show-toplevel` and `--git-dir` se actual repository discover karta hoon.
Nested `git init` se pehle parent repository and intended ownership/CI boundary verify
karta hoon.

## Easy-English minimum interview answer

**A Git repository stores version-control history and metadata for a defined file tree. A
project folder is an application boundary and may be inside a larger repository. I use
`git rev-parse --show-toplevel` to identify the actual repository root.**

Short version:

**A repository is Git's history and metadata boundary; it is not automatically the same
as an application folder.**

## Completion boundary

Topic 36 mein repository definition, metadata/discovery, bare/non-bare and boundary models
complete hue. **Topic 37 — Working tree** next hai aur abhi start nahi hua.

