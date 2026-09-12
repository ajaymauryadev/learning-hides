# Topic 44 — `git init`

## Learning goal

`git init` ka purpose, repository boundary, `.git` metadata directory, default branch,
re-initialization aur nested-repository risk samajhna hai. Current TaskForge files ko
damage kiye bina command ko isolated temporary folder mein verify karna hai.

## Simple definition

**`git init` ek normal folder ke andar Git repository metadata initialize karta hai.**

```text
normal folder + git init
        ↓
working tree + hidden .git metadata directory
```

Command application source code, README ya commit automatically create nahi karta.

## Command anatomy

```powershell
git init
```

```text
git   -> Git program
init  -> initialize subcommand
```

Git current working directory ko repository root banata hai, jab tak hum explicit path
na dein:

```powershell
git init taskforge-backend
```

Yeh second form named folder create/use karke usmein repository initialize kar sakta hai.
Command chalane se pehle current location aur intended boundary verify karna essential hai.

## `git init` kya create karta hai?

Typical non-bare repository mein hidden `.git` directory create hoti hai. Ismein Git ki
history aur configuration ke liye metadata hota hai:

```text
project-root/
|-- .git/
|   |-- HEAD
|   |-- config
|   |-- objects/
|   `-- refs/
`-- future-project-files
```

- `HEAD` current branch/reference identity batata hai.
- `config` repository-specific configuration rakh sakta hai.
- `objects/` commits, trees aur blobs store karega.
- `refs/` branches/tags ke references store karega.

In files ko beginner ko manually edit nahi karna chahiye.

## Initialization ke turant baad state

Fresh repository mein normally:

```text
repository exists
working tree exists
commits = 0
tracked project files = 0
remote = none
```

Isliye repository initialize hona aur initial commit exist hona separate events hain.
Initial commit curriculum ke Topic 57 mein hoga.

## Unborn branch

Fresh repository branch name show kar sakti hai, lekin first commit se pehle us branch ka
commit reference abhi exist nahi karta. Is state ko often **unborn branch** kaha jata hai.

```text
HEAD says: refs/heads/main
refs/heads/main commit: abhi nahi
```

First commit banne par actual branch reference commit ko point karega.

## Default branch explicitly choose karna

Machine ki Git configuration ke karan default branch name different ho sakta hai. Predictable
result ke liye modern Git mein branch name explicitly diya ja sakta hai:

```powershell
git init --initial-branch=main
```

`main` technical requirement nahi; team convention hai. Explicit option environment-dependent
surprise reduce karta hai.

## Bare versus non-bare repository

Normal development ke liye non-bare repository chahiye:

```powershell
git init --initial-branch=main
```

Bare repository server/shared storage use case ke liye hoti hai:

```powershell
git init --bare
```

Bare repository mein normal working tree nahi hota. TaskForge application code edit karne ke
liye `--bare` use nahi karenge.

## Current real repository boundary

Read-only evidence:

```text
Current repository root: C:/Users/ajaym/Desktop/Practicle
Current Git directory:   .git
Inside working tree:     true
```

`taskforge-backend/` empty child folder hai, lekin parent `Practicle` repository already usse
cover karti hai:

```text
Practicle/                 <- existing repository root
|-- .git/
`-- taskforge-backend/     <- parent working tree ka child
```

Is folder ke andar abhi `git init` chalane se nested repository ban jayegi. Yeh boundary
accidentally create karna architecture decision hota, simple tutorial step nahi. Isliye real
folder ko is topic mein mutate nahi kiya gaya.

## Nested repository risk

```text
parent/.git/
parent/child/.git/
```

Child `.git` milne ke baad child commands child repository use karenge. Parent Git child ke
contents ko ordinary files ki tarah manage na kare; staging par embedded-repository warning ya
gitlink-like behavior aa sakta hai. Submodule ek intentional advanced relationship hai—accidental
nested repository uska safe replacement nahi.

## Repository discovery flow

Folder ke andar Git command chalne par Git current location se parents ki taraf `.git` search
kar sakta hai:

```text
current folder
  ↓ no .git? inspect parent
parent folder
  ↓ .git found
that parent becomes repository root
```

Isliye empty `taskforge-backend/` ke andar `git status` chalana bhi current parent repository
ko refer karega, jab tak child ka own repository intentionally initialize na ho.

## Safe pre-initialization checks

```powershell
Get-Location
git rev-parse --show-toplevel
git rev-parse --is-inside-work-tree
Get-ChildItem -Force
```

`git rev-parse` repository ke bahar failure de sakta hai; woh useful evidence hai ki current
folder existing Git boundary ke andar nahi hai.

## Safe isolated practical

Real project boundary mutate karne ke bajay unique temporary folder mein yeh flow verify hua:

```powershell
New-Item -ItemType Directory -Path <unique-temp-folder>
Set-Location <unique-temp-folder>
git init --initial-branch=main
git rev-parse --is-inside-work-tree
git rev-parse --show-toplevel
git rev-list --all --count
```

Expected and verified meaning:

```text
true       -> folder Git working tree hai
temp path  -> wahi repository root hai
0          -> abhi koi commit nahi
```

Verification repository OS temporary location mein bana; workspace ke andar test folder retain
nahi hua. Real workspace HEAD, staging area, remote configuration aur `taskforge-backend/`
unchanged rahe. OS temporary folder automatic cleanup ke liye eligible hai; destructive cleanup
verification command ka part nahi banaya gaya.

## Re-running `git init`

Existing repository root par `git init` generally repository ko reinitialize karta hai; existing
history automatically erase nahi hoti. Phir bhi command casually repeat nahi karni chahiye:

- wrong folder mein run karke unwanted boundary ban sakti hai;
- supplied options/config expectations change kar sakte hain;
- nested repository accidentally create ho sakti hai;
- output success hona architecture correctness prove nahi karta.

## `git init` versus `git clone`

```text
git init  -> yahin new local repository metadata start karo
git clone -> existing repository ki local copy + history + common remote setup banao
```

Existing remote project join karte waqt generally `clone`; brand-new local history start karte
waqt `init` appropriate hota hai.

## Execution order

```text
intended project boundary decide
  -> current path verify
  -> parent repository detect
  -> init or clone choose
  -> explicit initial branch choose
  -> repository root verify
  -> status inspect (Topic 45)
```

## Common errors aur explanations

### `fatal: not a git repository`

Current folder aur parents mein usable `.git` metadata nahi mila. Correct project path par jao;
sirf error silence karne ke liye unknown folder mein `git init` mat chalao.

### `unknown option initial-branch`

Installed Git version option support na karti ho sakti hai. Version inspect karo. Older workflow
mein init ke baad branch rename possible hai, lekin woh later branch command knowledge maangta hai.

### `Reinitialized existing Git repository`

Command existing repository root mein repeat hui. History erase hone ka message nahi, lekin path
dobara verify karo aur samjho command kyun run hui.

### Embedded repository warning while staging

Likely child folder ka own `.git` hai. Parent/child repository relationship decide karo. `.git`
delete ya submodule commands blindly mat chalao—pehle contents/history preserve aur inspect karo.

### Permission denied

Git target location mein metadata write nahi kar saka. Folder permissions/ownership check karo;
administrator mode ko default fix mat banao.

## Data flow

`git init` network ya database operation nahi hai:

```text
terminal command
  -> Git process
  -> current filesystem folder
  -> local .git metadata
  -> repository initialized
```

No GitHub upload, remote creation, package installation or application process start hota hai.

## Student exercise

1. `git init` aur first commit mein kya difference hai?
2. TaskForge child folder mein directly `git init` kyun nahi chalaya?
3. Fresh repository mein commit count kya hoga?
4. `git init` aur `git clone` kab use honge?
5. Repository root inspect karne ka command likho.

## Exercise answers

1. `init` metadata/boundary banata hai; commit staged snapshot ko history mein record karta hai.
2. Parent `Practicle` already repository hai; child init accidental nested boundary bana sakta tha.
3. Zero, jab tak first commit create na ho.
4. New local repository ke liye init; existing repository copy karne ke liye clone.
5. `git rev-parse --show-toplevel`.

## Interview question with Hinglish answer

**Question:** `git init` kya karta hai?

**Answer:** `git init` current ya specified folder mein Git repository initialize karta hai. Yeh
`.git` metadata create karta hai, lekin project files, commit aur remote automatically create nahi
karta. Command run karne se pehle main path aur existing parent repository check karta hoon, taaki
accidental nested repository na बने।

## Easy-English minimum interview answer

> `git init` initializes a new local Git repository in a folder. It creates Git metadata, but it
> does not create a commit or a remote repository. I verify the folder first to avoid creating an
> accidental nested repository.

## Completion boundary

Topic 44 mein initialization concept aur isolated execution verify hui. Real TaskForge repository
boundary intentionally unchanged hai. Topic 45 — `git status` abhi start nahi hua.
