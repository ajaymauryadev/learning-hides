# Topic 33 — Project root folder create karna

## Learning goal

TaskForge backend ke liye exact, safely verified project root directory create karni hai;
project root ko CWD, VS Code workspace root and Git root se distinguish karna hai. Phase 1
ka required practical output isi topic mein actual filesystem par banega.

## Final practical output

```text
C:\Users\ajaym\Desktop\Practicle\taskforge-backend\
```

Verified current state:

```text
Exists: True
Type: Directory
Name: taskforge-backend
Child count: 0
```

Folder intentionally empty hai. `package.json`, source, tests or configuration future
ordered topics mein real need par create honge.

## Project root kya hota hai?

**Project root application ke files/directories ka intended top-level container hota
hai.** Future TaskForge structure isi ke andar grow hogi:

```text
taskforge-backend/             <- project root
  package.json                 <- later
  src/                         <- later
  tests/                       <- later
  configuration/docs           <- later as justified
```

Project root sirf random folder nahi; project-relative paths, tooling and developer
workflow ka stable boundary hota hai.

## Root word ka context

“Root” multiple contexts mein use hota hai:

```text
Filesystem drive root -> C:\
Current workspace root -> VS Code opened top-level folder
Git root              -> repository owning .git metadata
Project root          -> application top-level folder
API route root        -> HTTP routing context
```

Topic 33 ka `project root` filesystem drive root `C:\` nahi. Broad system root par
project files create/delete nahi karne.

## Parent and child relationship

```text
Parent:
C:\Users\ajaym\Desktop\Practicle

Child/project root:
C:\Users\ajaym\Desktop\Practicle\taskforge-backend
```

Child parent boundary ke andar exact one-level directory hai.

## Folder naming decision

Name:

```text
taskforge-backend
```

Reasons:

- product + component responsibility clear;
- lowercase avoids cross-platform casing confusion;
- hyphen readable and shell-friendly;
- spaces absent, so common commands simpler;
- roadmap practical output se exact match.

`TaskForge Backend`, `backend-final`, `new-folder` jaisi inconsistent names later paths,
CI and documentation mein confusion create kar sakti hain.

## Preflight before creation

Creation se pehle:

```text
Parent=C:\Users\ajaym\Desktop\Practicle
Target=C:\Users\ajaym\Desktop\Practicle\taskforge-backend
ParentExists=True
TargetExists=False
```

Target absence important thi. Existing same-name item hota to overwrite/merge/Force
assume nahi karte; inspect karke user intent resolve hota.

## Creation command

```powershell
New-Item -ItemType Directory -Path ".\taskforge-backend"
```

Anatomy:

```text
New-Item               -> create operation
-ItemType Directory    -> folder create karni hai
-Path                  -> target parameter
.\taskforge-backend     -> CWD-relative target
```

`-Force` intentionally use nahi hua because collision ko silently overlook nahi karna.

## Safe creation algorithm

```text
1. Get-Location se intended parent verify
2. exact child name define
3. normalized absolute target calculate
4. target parent boundary ke andar assert
5. parent directory exists assert
6. target absent assert
7. one directory create
8. Resolve-Path se exact location verify
9. item type directory verify
10. expected initial contents verify
```

## Verification commands

```powershell
Test-Path -LiteralPath ".\taskforge-backend" -PathType Container
Resolve-Path -LiteralPath ".\taskforge-backend"
Get-Item -LiteralPath ".\taskforge-backend"
Get-ChildItem -LiteralPath ".\taskforge-backend" -Force
```

Actual:

```text
Created=True
ResolvedPath=C:\Users\ajaym\Desktop\Practicle\taskforge-backend
IsDirectory=True
ChildCount=0
```

## Empty folder kyun?

Curriculum one topic at a time:

- Topic 33 output project root folder only;
- npm/project manifest later npm foundation mein;
- source files backend JS/Node topics mein;
- `.gitignore` Phase 2 ordered Git topic mein;
- no placeholder complexity without responsibility.

Empty is intentional state, incomplete verification nahi.

## Empty directory and Git

Git normally empty directories track nahi karta; files/snapshots track karta hai. Isliye
`git status` new empty `taskforge-backend/` ko show na kare to folder absent nahi.

```text
Filesystem check -> folder exists
Git status       -> no file to track inside empty folder
```

Artificial `.gitkeep` Git feature nahi and Topic 33 requirement nahi, so create nahi hua.
First justified project file later folder ko repository snapshot mein represent karegi.

## Critical current Git-root evidence

Current repository Git top-level:

```text
C:/Users/ajaym/Desktop/Practicle
```

Inside new child directory:

```text
Inside Git work tree: True
Reported Git top-level: C:/Users/ajaym/Desktop/Practicle
Child has own .git: False
```

Therefore `taskforge-backend/` currently parent Git repository ke work tree ke andar ek
project folder hai, independent Git repository nahi.

## Nested repository warning

Child mein blindly `git init` chalane se nested repository metadata ban sakti hai:

```text
Practicle/.git                 <- existing repository
Practicle/taskforge-backend/.git <- potential nested repository
```

Nested repository advanced/special layout ho sakti hai and parent tracking behavior
complicate karegi. Phase 2 Topic 34 onward version-control mental model complete karne ke
baad repository-boundary decision lenge. Topic 33 ne `git init` nahi chalaya.

## Project root versus CWD

Folder exist hona current shell automatically usmein enter hona nahi:

```text
Current shell CWD:
C:\Users\ajaym\Desktop\Practicle

Created project root:
C:\Users\ajaym\Desktop\Practicle\taskforge-backend
```

Enter karne ke liye future command:

```powershell
Set-Location -LiteralPath ".\taskforge-backend"
```

Topic verification ne original CWD stable rakhi. Future project commands se pehle
`Get-Location` verify hoga.

## Project root versus VS Code workspace

Creating folder does not automatically make it active VS Code workspace:

```text
Folder created on disk
  != VS Code opened it
  != terminal CWD changed
```

Focused development mein folder VS Code mein open ki ja sakti hai, then integrated
terminal CWD separately verify karenge. Topic 33 ne GUI/window state change nahi ki.

## Project root versus package root

npm commonly `package.json` containing directory ko package root/context maanta hai.
Currently:

```text
Project root directory exists: True
package.json exists inside: False
npm package initialized: False
```

So “folder created” ko “Node/npm project initialized” nahi bolenge.

## Project root versus running server

```text
Directory exists
  != source code exists
  != Node process running
  != port listening
  != API available
```

Phase 1 output development container/boundary hai, backend feature/server nahi.

## Common errors aur fixes

### Wrong CWD

Folder unintended parent mein create ho sakti hai. `Get-Location` and resolved absolute
target before/after verify.

### Target already exists

File or directory collision. Exact type/contents inspect; `-Force`/delete blindly nahi.

### Permission denied

Parent write permission issue. Administrator mode first fix nahi; intended location and
ownership confirm.

### Typo or spaces/casing mismatch

Roadmap name copy/compare and quote paths with spaces. Exact lowercase hyphenated name use.

### Folder exists but Git does not show it

Empty directory Git tracks nahi. `Test-Path` proves filesystem state; later justified file
Git can track.

### `npm` command wrong parent se run

CWD verify and package manifest location inspect. Folder existence package initialization
proof nahi.

### Nested `git init`

Parent Git root already exists. Repository model/boundary decide before initialization.

## Phase 1 practical output verification

Phase 1 required:

```text
taskforge-backend/
```

Actual:

```text
C:\Users\ajaym\Desktop\Practicle\taskforge-backend\
```

Acceptance evidence:

- exact intended name;
- exact intended parent;
- directory type;
- empty initial state;
- no collision overwritten;
- no nested Git repository;
- no package/source/config created;
- parent Git repository preserved.

## Student exercise

Without creating another folder, current result inspect and answer:

1. absolute project-root path kya hai?
2. parent directory kya hai?
3. project root and current CWD same hain?
4. `package.json` exists?
5. child ka own `.git` exists?
6. parent Git top-level kya hai?
7. empty folder Git status mein kyun absent ho sakti hai?
8. future project command se pehle kya verify karoge?

## Exercise answer

```text
Root: C:\Users\ajaym\Desktop\Practicle\taskforge-backend
Parent/CWD currently: C:\Users\ajaym\Desktop\Practicle
Root != current CWD
package.json: absent
child .git: absent
Git top-level: C:\Users\ajaym\Desktop\Practicle
Git tracks files/snapshots, not empty directories
future check: Get-Location + expected project files
```

## Interview question with Hinglish answer

**Question:** Project root folder kya hota hai aur safely kaise create/verify karte ho?

**Answer:** Project root application ka top-level directory boundary hai jiske andar
manifest, source, tests and config organize hote hain. Main CWD and intended parent verify,
exact normalized target check, collision inspect, then scoped directory create karta hoon.
After creation `Resolve-Path`, `Test-Path -PathType Container` and child listing se name,
type and contents verify karta hoon. Project root ko Git root, workspace root or npm
package root automatically assume nahi karta.

## Easy-English minimum interview answer

**A project root is the top-level folder that contains an application's files. Before
creating it, I verify the parent path and check for collisions. After creation, I verify
the resolved path, directory type, and expected contents.**

Short version:

**The project root is the main folder for the application. It is not automatically the
Git root, editor workspace, or npm package root.**

## Completion boundary

Topic 33 and Phase 1 complete hue. Only project root folder created; Phase 2 Git work
start nahi hua. **Next ordered topic: Phase 2, Topic 34 — Version control kya hai?**

