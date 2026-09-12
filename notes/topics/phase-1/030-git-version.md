# Topic 30 — Git version

## Learning goal

Installed Git CLI version, resolved executable and build identity verify karni hai; Git
ko GitHub/remote service se distinguish karna, multiple installations/PATH selection and
version-check evidence limits samajhne hain. VS Code version Topic 31 mein separately
verify hogi.

## Git kya hai?

**Git distributed version control system hai jo files ke changes, commits, branches and
history manage karta hai.**

```text
Working files
  -> staging/index
  -> local commits/history
  -> optional remote repository synchronization
```

Git installed locally work kar sakta hai; every operation internet require nahi karti.

## Git versus GitHub

```text
Git     -> local/distributed version-control tool
GitHub  -> remote hosting/collaboration service using Git repositories
```

`git --version` Git CLI verify karta hai, GitHub login/network/repository permissions
nahi.

## Primary version command

```powershell
git --version
```

Verified:

```text
git version 2.53.0.windows.2
exit code: 0
```

Output prefix human-readable product name hai; version identity
`2.53.0.windows.2` hai.

## Version anatomy

```text
2.53.0.windows.2
| |  |    |    |
| |  |    |    +-- Windows packaging/build revision
| |  |    +------- platform distribution marker
| |  +------------ upstream patch
| +--------------- upstream minor
+----------------- upstream major
```

Base version common `MAJOR.MINOR.PATCH` pattern `2.53.0` follow karti hai. Additional
`.windows.2` Git for Windows distribution/build-specific suffix hai. Exact vendor suffix
semantic upstream patch jaisa assume nahi karna.

## Command resolution

```powershell
Get-Command -Name git -All
```

Current PowerShell ne two executables discover kiye:

```text
Selected first:
C:\Program Files\Git\cmd\git.exe

Additional:
C:\Users\ajaym\.cache\codex-runtimes\...\native\git\cmd\git.exe
```

Normal `git` invocation first resolved command use karti hai. Multiple copies automatically
error nahi, but different shells/users/processes different Git choose kar sakte hain.

## Why path matters

```text
Same command name: git
  -> PATH/search precedence
  -> selected git.exe
  -> selected executable's version/config/runtime behavior
```

`git --version` alone selected version batata hai. `Get-Command git -All` ambiguity and
alternate installations reveal karta hai.

## Build-options evidence

```powershell
git version --build-options
```

Verified selected build highlights:

```text
git version: 2.53.0.windows.2
cpu: x86_64
built from commit: e9edee0b34751bf4d7d1feda0e2535bff64d4e77
default-ref-format: files
default-hash: sha1
feature: fsmonitor--daemon
```

Build output mein libraries/features bhi report hue. Beginner ko sab memorize nahi
karna; troubleshooting mein exact build/platform capability identify karne ke kaam aata
hai.

## `x86_64` versus Node `x64`

Git build `x86_64`, Node `x64` report kar raha hai. Dono current context mein 64-bit
x86 architecture naming conventions hain. String names tool-specific ho sakte hain.

## Build hash versus repository commit

`built from commit` Git program ko build karne wale source revision ko identify karta hai.
Woh TaskForge repository ka current commit nahi.

TaskForge commit inspect karne ke commands separate hain:

```powershell
git log -1 --oneline
git status --short
```

Do hashes/contexts mix nahi karna.

## Current repository context

Current Git repository already exists. Recent committed history begins with:

```text
f96332e docs: complete phase 1 topic 18 terminal
```

Topics 19 onward ke learning changes currently uncommitted hain and preserve karne hain.
Version verification ne commit, branch, remote or working files mutate nahi kiye.

## Git version check kya prove karta hai?

Proves:

- current shell Git command resolve kar sakti hai;
- selected Git executable launches;
- selected CLI reports `2.53.0.windows.2`;
- build platform/details inspectable hain;
- commands exit `0`.

Does not prove:

- repository clean hai;
- commits correct hain;
- Git user name/email configured hain;
- remote authentication works;
- `git push` permission/network works;
- current branch desired hai;
- all hooks/tools compatible hain;
- Git version latest or best hai.

## Git configuration scopes

Conceptual scopes:

```text
System config -> machine-wide
Global config -> current user
Local config  -> current repository
Command flags/env -> invocation-specific override
```

Version and configuration separate layers hain. Git command executable correct hone par
bhi config/permissions warning aa sakti hai.

## Current global-ignore warning

`git status --short` currently warns:

```text
unable to access 'C:\Users\ajaym/.config/git/ignore': Permission denied
```

Meaning selected Git user-level ignore path access nahi kar pa raha. Version command
still succeeds; repository status also prints, but global ignore behavior incomplete ho
sakta hai. This proves:

```text
CLI identity check PASS
global ignore configuration access WARNING
```

These results can coexist. Warning Topic 26 and `notes/DEBUG_LOG.md` mein recorded hai.
Outside-workspace global file/permission current topic mein change nahi hui.

## Local versus remote operations

Local/read-oriented examples:

```text
git --version
git status
git diff
git log
```

Remote/network examples:

```text
git fetch
git pull
git push
```

Version verification remote authorization nahi. Push user provided Git commands mein
rahega; Topic 30 ne remote mutation perform nahi ki.

## Git executable and shell helpers

Git for Windows Unix-like helper shell/tools bundle kar sakta hai. Build options showed:

```text
shell-path: D:/git-sdk-64/usr/bin/sh
```

Yeh current interactive shell ko Git Bash prove nahi karta. Current shell process still
`pwsh`; build field Git ke internal/build helper path ka metadata hai.

## Compatibility decisions

Git command behavior/version support scripts, CI providers, credential helpers and
platform features se relate kar sakta hai. Production/team decision:

```text
installed Git version
  + team-required commands/features
  + CI/deployment Git version
  + security/support policy
  -> compatibility decision
```

Current “latest” status time-sensitive hai; official release/security source verify kiye
bina newest claim nahi.

## Multiple Git installations diagnose karna

```powershell
Get-Command git -All
git --version
```

Different terminal mein repeat:

```text
Terminal A -> path/version A
Terminal B -> path/version B
```

If behavior differs:

1. CWD compare;
2. selected command path compare;
3. version compare;
4. config scopes/origin compare;
5. environment/PATH compare;
6. one intentional change, then reverify.

Duplicate executable blindly delete nahi.

## Common errors aur fixes

### `git` not recognized

Installation, spelling, PATH and fresh terminal inspect. `Get-Command git -All` evidence.

### Wrong Git version selected

Multiple executables and PATH order inspect. Full path/version pair record.

### Version works, status says not a repository

Git CLI installed hai but current CWD Git work tree ke inside nahi. `Get-Location` and
intended repository root verify.

### Version works, push fails

Remote URL, credentials, permissions, branch rules, network separate layers hain. Version
reinstall default fix nahi.

### Permission/config warning

Exact config path/scope inspect. Administrator mode or security weakening blindly nahi.

### “Dubious ownership” safety error

Repository ownership/trust mismatch ho sakta hai. Target ownership and why directory is
trusted establish kiye bina global safe-directory exception add nahi.

### Newer Git changed behavior

Release notes/project scripts/CI version compare; reproducible command and output capture.

## Safe verification checklist

```text
1. Get-Location
2. Get-Command git -All
3. git --version
4. git version --build-options
5. native exit codes check
6. repository status separately inspect
7. warnings/errors preserve and classify
8. required feature/team/CI compatibility compare
9. remote health only when required and authorized
10. no config/update mutation during identity check
```

## TaskForge Git record

```text
Selected Git: C:\Program Files\Git\cmd\git.exe
Version: 2.53.0.windows.2
CPU/build architecture: x86_64
Version/build command exits: 0
Second Git executable: present in Codex bundled runtime path
Repository: present
Global ignore config: permission warning remains
```

No Git install/update, config change, commit, checkout, pull or push performed.

## Student exercise

Fill:

```text
git --version:
selected path:
all resolved paths:
build CPU:
build commit:
exit codes:
repository status result:
warnings:
```

Explain:

1. Git and GitHub difference.
2. `.windows.2` kya indicate karta hai?
3. Multiple Git executables kyun matter karte hain?
4. Version pass but push fail kaise possible?
5. Build commit and TaskForge commit kyun different concepts hain?

## Exercise answer

```text
Version: git version 2.53.0.windows.2
Selected: C:\Program Files\Git\cmd\git.exe
Alternate: Codex bundled runtime Git path
CPU: x86_64
Build commit: Git program build source revision
Exits: 0
Warning: inaccessible user/global ignore path
Git is VCS; GitHub is remote hosting/collaboration
.windows.2 is distribution/build suffix
multiple paths can select different versions
push depends on remote/network/auth, not only CLI version
TaskForge commit belongs to repository history, not Git binary build
```

## Interview question with Hinglish answer

**Question:** Git version ko reliably kaise verify karte ho?

**Answer:** Main `git --version` se selected CLI version, `Get-Command git -All` se all
resolved executable paths and PATH selection, aur `git version --build-options` se build
identity/platform inspect karta hoon. Version success only local Git executable ko prove
karta hai; repository state, configuration, GitHub authentication, network and push
permissions separate checks hain. Warnings ko version failure ke saath mix nahi karta.

## Easy-English minimum interview answer

**I verify Git with `git --version`, inspect all resolved executable paths, and check the
build information. A successful version check proves the local Git CLI runs, but remote
authentication, repository state, and push access require separate checks.**

Short version:

**`git --version` shows the selected local Git version. GitHub access and repository
health are separate concerns.**

## Completion boundary

Topic 30 mein Git version/build identity, Git versus GitHub, multiple resolution,
configuration warnings and compatibility boundaries complete hue. **Topic 31 — VS Code
version** next hai aur abhi start nahi hua.

