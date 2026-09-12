# Topic 45 — `git status`

## Learning goal

`git status` se current branch, upstream summary, staged changes, unstaged changes aur
untracked files ko safely read karna hai. Long, short aur porcelain output samajhna hai.

## Simple definition

**`git status` repository ki current working state ka summary dikhata hai.**

```text
HEAD snapshot
     ↕ compare
staging area
     ↕ compare
working tree
```

Yeh history record, files stage, commit ya remote par upload nahi karta.

## Basic command

```powershell
git status
```

```text
git     -> Git program
status  -> current repository state inspect karne ka subcommand
```

Default output human-readable explanation aur suggested commands deta hai.

## `git status` kya inspect karta hai?

Main questions:

1. Current branch kaunsi hai?
2. Configured upstream ke comparison ka cached summary kya hai?
3. Index aur `HEAD` mein kya difference hai—kya staged hai?
4. Working tree aur index mein kya difference hai—kya unstaged hai?
5. Kaunse files untracked hain?

Ignored files default status mein normally hide rehti hain.

## Current real long-status evidence

Topic verification ke start par output ka meaning:

```text
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  modified: LEARNING_MEMORY.md
  modified: notes/ARCHITECTURE.md
  modified: notes/BACKEND_ROADMAP.md
  modified: notes/LEARNING_STATE.md

Untracked files:
  notes/topics/phase-2/

no changes added to commit
```

Interpretation:

- current local branch `main` hai;
- local cached `origin/main` comparison ahead/behind nahi dikhata;
- four tracked files working tree mein modified hain, staged nahi;
- Phase 2 folder ke andar untracked files hain;
- staging area mein current work se koi change selected nahi.

`up to date` live GitHub check nahi hai. `git status` local remote-tracking ref se compare karta
hai; current server state jaanne ke liye later explicit network synchronization knowledge chahiye.

## Short status

```powershell
git status --short
```

Observed form:

```text
 M LEARNING_MEMORY.md
 M notes/ARCHITECTURE.md
 M notes/BACKEND_ROADMAP.md
 M notes/LEARNING_STATE.md
?? notes/topics/phase-2/
```

Short output ka general format:

```text
XY path
```

- `X` first column: index/staging area ka `HEAD` se relation.
- `Y` second column: working tree ka index se relation.

## Important short-status codes

```text
 M file  -> tracked file modified in working tree; unstaged
M  file  -> modification staged in index
MM file  -> version staged bhi hai, staging ke baad aur modification bhi hai
A  file  -> new file staged
 D file  -> tracked file working tree se deleted; deletion unstaged
D  file  -> deletion staged
R  file  -> rename staged/detected for status presentation
?? file  -> untracked
!! file  -> ignored, only when ignored output requested
```

Spaces meaningful hain. ` M` aur `M ` different states hain.

## Directory summary ka important detail

Status ne yeh dikhaya:

```text
?? notes/topics/phase-2/
```

Iska meaning sirf ek untracked file nahi. Git human-friendly output mein entire untracked
directory collapse kar sakta hai. Separate read-only enumeration se currently 11 untracked topic
files मिले। Isliye status line count aur actual file count same hona required nahi.

Individual untracked paths inspect karne ke liye:

```powershell
git ls-files --others --exclude-standard
```

## Branch summary ke saath short output

```powershell
git status --short --branch
```

Observed header:

```text
## main...origin/main
```

Yeh current branch/upstream relationship ka compact local summary hai. Divergence hone par
`[ahead N, behind M]` jaisa detail show ho sakta hai.

## Porcelain output

```powershell
git status --porcelain=v1
```

Porcelain format scripts/tools ke liye stable parseable output provide karta hai. Current output
short form jaisa tha. Human lesson ke liye normal/short status; automation ke liye porcelain
choose karna safer hai. Human prose ko script se parse nahi karna chahiye.

## Clean working tree

Clean output ka typical meaning:

```text
nothing to commit, working tree clean
```

Iska matlab tracked working/index state `HEAD` se match karti hai aur visible untracked changes
nahi hain. Yeh prove nahi karta ki:

- remote server ka live state same hai;
- ignored files absent hain;
- application tests pass hain;
- deployed code current hai.

## Ignored files inspect karna

```powershell
git status --short --ignored
```

Ignored entries `!!` se show ho sakti hain. Current verification mein ignored entry count zero
tha. `.gitignore` ka creation aur rule behavior Topic 46 hai.

## `git status` execution/data flow

```text
run command in folder
  -> Git discovers repository root
  -> reads HEAD and index
  -> scans relevant working-tree paths
  -> compares states
  -> prints summary
```

No request TaskForge API/database ko nahi jati. Normal status local repository inspection hai.

## Recommended inspection order

```powershell
git status --short --branch
git status
git diff
git diff --cached
```

Status batata hai **which state/path category** changed. Diff batata hai **content exactly kya
changed**. Diff commands Topics 48–49 mein detail se aayenge.

## Current state counts

Read-only supporting commands se verification-start state:

```text
modified tracked, unstaged: 4
staged paths:                0
untracked individual files: 11
ignored entries:             0
```

Lesson file add hone ke baad untracked individual file count naturally 12 ho jayega. Isse samajh
aata hai ki status ek time-specific snapshot/report hai; later file operations state change karte
hain.

## Common errors aur explanations

### `fatal: not a git repository`

Git current folder aur parents mein repository metadata discover nahi kar saka. `Get-Location`
aur intended project root verify karo. Unknown location mein sirf error हटाने के लिए `git init`
mat chalao.

### ` M` ko staged samajhna

Second-column `M` unstaged modification hai. Staged modification `M ` hoti hai. Spaces preserve
karke `XY` model read karo.

### `?? folder/` ko one file samajhna

Human status untracked directory collapse kar sakta hai. Required ho to individual untracked
paths separately enumerate/inspect karo.

### `up to date` ko live remote proof samajhna

Status cached remote-tracking ref compare karta hai. Network fetch ke bina actual remote branch
newer ho sakti hai.

### Clean status ko tested/deployed samajhna

Clean Git state sirf repository comparisons batati hai. Tests, runtime aur deployment separate
states hain.

### Suggested discard command blindly chalana

Long status suggestions context-dependent hain. `git restore` local work discard kar sakta hai;
content inspect aur recovery need samjhe bina mat chalao.

## Student exercise

Current short status ko dekhkar answer karo:

```text
 M notes/ARCHITECTURE.md
?? notes/topics/phase-2/
```

1. Architecture change staged hai ya unstaged?
2. `??` ka kya meaning hai?
3. Kya `?? folder/` exactly one file prove karta hai?
4. Status aur diff ka basic difference kya hai?
5. Stable scripting output ke liye kaunsa command use karoge?

## Exercise answers

1. Unstaged, kyunki `M` second column mein hai.
2. Path untracked hai.
3. Nahi; directory ke andar multiple untracked files ho sakti hain.
4. Status categories/paths summarize karta hai; diff exact content changes dikhata hai.
5. `git status --porcelain=v1`.

## Interview question with Hinglish answer

**Question:** `git status` kya batata hai?

**Answer:** `git status` current branch aur working tree, staging area aur last commit ke beech
changes ka summary dikhata hai. Isse staged, unstaged aur untracked files identify hoti hain. Yeh
files stage ya commit nahi karta. Short format mein first column index aur second column working
tree ko represent karti hai.

## Easy-English minimum interview answer

> `git status` shows the current branch and summarizes staged, unstaged, and untracked changes.
> It does not stage or commit files. In short format, the first column describes the index and the
> second column describes the working tree.

## Completion boundary

Topic 45 mein repository state inspection verify hui. Koi file staged, committed, restored ya
deleted nahi hui. Topic 46 — `.gitignore` abhi start nahi hua.
