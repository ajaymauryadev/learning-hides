# Topic 35 — Git kya hai?

## Learning goal

Git ko version control concept ka concrete tool samajhna hai: distributed model, local
history, main areas, content-based records, branches/remotes ka overview, capabilities and
limitations. Individual repository states and commands Topics 36–57 mein one-by-one.

## Simple definition

**Git ek distributed version control system hai jo project files ke changes aur history
ko locally record/manage karta hai, parallel development support karta hai, aur optional
remote repositories ke saath history exchange kar sakta hai.**

```text
Version control = general problem/system category
Git             = us category ka specific tool
```

Jaise database general category hai aur MongoDB specific technology, waise version
control category hai aur Git specific implementation.

## Git naam ka role

Terminal mein Git CLI commands `git` program se start hoti hain:

```powershell
git --version
git status
git log
```

First token `git` executable hai; next token subcommand. Topic 35 mein identity/context
inspection read-only hua. State-changing subcommands ordered topics se pehle use nahi.

## Git distributed kyun kehlata hai?

Distributed VCS mein normal repository clone ke paas project files ke saath substantial
local history/object database hoti hai. Developer many history operations offline/local
kar sakta hai:

```text
Developer A repository + local history
           ↕ explicit synchronization
Shared remote repository
           ↕ explicit synchronization
Developer B repository + local history
```

Remote unavailable ho to local edits, comparisons and commits possible ho sakte hain.
Share/synchronize ke liye remote/network operations separately required hain.

## Centralized versus distributed beginner comparison

```text
Centralized model:
developer working copy -> central history server

Distributed Git model:
developer repository/history <-> other repository/remote
```

Real workflows policies and hosting tools use karte hain; distributed ka meaning “no
coordination” nahi. Teams still protected branches, reviews, CI and access rules use karte
hain.

## Git aur GitHub same nahi

```text
Git     -> local/distributed VCS software and data model
GitHub  -> Git repository hosting/collaboration platform
```

Git without GitHub work kar sakta hai. GitHub alternative remotes bhi exist karte hain.
`git --version` GitHub account/authentication prove nahi karta.

## Git ke main conceptual areas

Detailed topics later, overview now:

```text
Working tree
  -> developer-visible current files

Staging area / index
  -> next commit ke liye selected snapshot

Local repository
  -> committed objects/history and references

Remote repository
  -> another repository mapping used for explicit exchange
```

Flow preview:

```text
edit files -> inspect -> select/stage -> review -> commit locally -> push later
```

Git automatically every saved change commit nahi karta.

## Working tree overview

Working tree current checked-out project files/directories ka filesystem view hai. Editor
and application isi state ko read karte hain. It can differ from last committed state.
Formal Topic 37 mein.

## Staging area overview

Staging area/index next commit ke intended content selection ka intermediate state hai.
It supports “file mein jo abhi disk par hai” and “next commit mein jo selected hai” ko
different rakhna. Formal Topic 40 mein.

## Local repository overview

`.git/` metadata directory normally object database, references and repository config
hold karti hai. Commits local repository history mein create hote hain; commit automatically
GitHub par upload nahi. Repository Topic 36 and commit Topic 41.

## Remote overview

Remote another Git repository ka named connection/mapping hota hai, often hosting service
URL. Remote is not mandatory for local Git. Topic 43/52 onward detail.

## Git content ko kaise sochta hai?

Beginner mental model:

```text
file contents -> Git objects/snapshots
directory structure -> tree-like snapshot
snapshot + parents + author/message metadata -> commit
branch name -> commit history position ka movable reference
```

Git internal details optimize/store content efficiently. User ko “entire folder duplicate
every commit” manually manage nahi karna.

## Content-addressed identity

Git objects content-derived hashes se identify hote hain. Content change hone par object
identity change ho sakti hai. Hash helps integrity/reference identity, but:

- hash encryption nahi;
- secret safe nahi hota;
- short hash collision-free universal guarantee nahi;
- commit hash ko human meaning dene ke liye message/context required.

Exact object internals later advanced Git learning mein deepen honge.

## Commit graph overview

Commits parent relationships se history graph banate hain:

```text
A <- B <- C
      \ <- D
```

Straight line possible, branches/merges graph create karte hain. History simple file
version number list se richer ho sakti hai.

## Git files track karta hai, empty directories nahi

Current `taskforge-backend/` empty hai, isliye parent Git status mein entry nahi. Git
snapshot tracked file content/tree relationships represent karta hai; standalone empty
directory normally record nahi.

First justified file later folder ko snapshot structure mein represent karegi. Fake
placeholder Topic 35 mein create nahi.

## Rename detection nuance

Git generally file history/content changes record karta hai; rename often old path deletion
+ new path addition similarity se detect/present hota hai, special permanent “rename event”
as beginner might imagine zaroori nahi. Practical rename/diff behavior later observe hoga.

## Git kya kar sakta hai?

- current files versus recorded state compare;
- intentional snapshot/history record;
- history inspect;
- branches ke through parallel work;
- changes merge/rebase-like workflows se integrate;
- other repositories/remotes se exchange;
- authorship/time/message metadata retain;
- regression investigation/bisect-like analysis support.

Each operation correct usage and verification maangti hai.

## Git kya automatically nahi karta?

- code correct/tested prove;
- meaningful commit message write;
- business conflict resolve;
- secret detect/prevent guaranteed;
- database backup;
- GitHub authentication;
- deployment;
- remote sync automatically;
- uncommitted/untracked lost work recover guaranteed;
- malicious code safe declare.

## Git local-first behavior

Many commands local data use karte hain:

```text
status/diff/log/commit -> commonly local repository/working state
fetch/pull/push        -> remote/network interaction
```

Exact behavior/options future topics. Local commit and remote push separate events hain.

## Git configuration

Git behavior system/global/local configuration scopes se influence ho sakta hai: identity,
defaults, ignore path, line endings, credential helpers etc. Installed binary version and
configuration separate layers.

Current global-ignore access warning isi distinction ka evidence hai: Git executable works,
but one user-level config path access warning exists.

## Git hooks overview

Repository/client/server hooks certain events par scripts run kar sakte hain. They can
automate checks but:

- hooks untrusted code execute kar sakte hain;
- local hooks every clone mein automatically shared guarantee nahi;
- hook pass correctness/security proof complete nahi;
- bypass/policy and CI separate layers.

TaskForge real validation need par tooling introduce karega.

## Git and line endings

Windows commonly CRLF, Linux commonly LF line endings use karte hain. Git configuration
and attributes conversions/diffs influence kar sakte hain. Current `git diff --check`
warnings mention working-copy LF/CRLF conversion; it is warning/context, syntax failure
nahi. Cross-platform policy later repository configuration need par set hogi, blindly
global config change nahi.

## Current verified Git identity

```text
Git version: 2.53.0.windows.2
Resolved executable: C:\Program Files\Git\cmd\git.exe
Inside work tree: true
Git top-level: C:/Users/ajaym/Desktop/Practicle
Git directory: .git
Current branch: main
```

Inside `taskforge-backend/`:

```text
Reported Git top-level: C:/Users/ajaym/Desktop/Practicle
Own .git: False
```

This proves child currently belongs to parent work-tree context, independent repository
nahi.

## Topic 35 read-only command flow

```text
PowerShell resolves git.exe
  -> Git reads version or current repository metadata
  -> output returns
  -> no index/commit/branch/remote mutation
```

## Common mistakes aur fixes

### Git = GitHub

Tool and hosting service separately identify; local history vs remote synchronization
explain.

### Save = commit

Saving disk working file update karta hai. Commit requires intentional selection/record.

### Commit = push

Commit local history record; push remote exchange. Separate evidence.

### Git tracks every file automatically

Untracked/ignored/state concepts matter. Topics 38–39/46.

### Git folder backup automatically

Empty/untracked/ignored/external data missing ho sakta hai; dedicated backups separate.

### `.git` manually edit/delete

Repository metadata damage risk. Git commands and recovery understanding use.

### Child folder mein blindly `git init`

Parent root exists. Desired repository boundary understand before nested metadata create.

### Hash means secret encrypted

Git history content retrievable ho sakta hai. Secrets never commit; exposure response
Topic 56.

## Safe Git learning sequence

```text
Version control purpose
  -> Git tool
  -> repository and working states
  -> staging/commit/branch/remote models
  -> init/status/ignore/add/diff/commit/log/remote/sync commands
  -> safe workflow and secrets
  -> reviewed initial commit
```

This is Phase 2 ordered curriculum; commands concepts se pehle blindly memorize nahi.

## TaskForge connection

Git TaskForge ke source, tests, safe configuration and docs ko evolve karne mein help
karega:

```text
feature requirement
  -> scoped implementation + tests/docs
  -> diff review
  -> meaningful local commit
  -> later remote review/CI
```

Git database records version-control history; TaskForge MongoDB business data store nahi.

## Student exercise

1. Git and version control difference bolo.
2. Git and GitHub difference bolo.
3. Working tree, staging area, local repository, remote ka one-line overview.
4. Commit vs push distinguish karo.
5. Git empty folder kyun show nahi karta?
6. Git kya prove nahi karta—four examples.
7. Current child folder ka Git root and own `.git` state explain.

## Exercise answer

```text
Version control = category; Git = distributed VCS tool
Git = local/distributed software; GitHub = hosting/collaboration service
working tree = current files
staging = next commit selection
local repository = committed history/metadata
remote = another repository mapping
commit local record; push remote exchange
empty folder has no tracked file snapshot
Git does not prove tests, security, backup or deployment
child inherits Practicle Git root and has no own .git
```

## Interview question with Hinglish answer

**Question:** Git kya hai aur distributed version control ka kya meaning hai?

**Answer:** Git ek distributed version control system hai jo project changes aur commit
history locally manage karta hai. Normal clone mein project ke saath local history hoti
hai, so many status/diff/commit/history operations remote ke bina ho sakte hain. Remote
repositories explicit fetch/push/pull se exchange hote hain. Git GitHub se separate hai
and code correctness, secrets protection, backups or deployment automatically guarantee
nahi karta.

## Easy-English minimum interview answer

**Git is a distributed version control system. It stores project history locally, supports
parallel development, and can exchange commits with remote repositories. Git is the tool;
GitHub is one service that can host Git repositories.**

Short version:

**Git tracks project history locally and lets developers collaborate through branches and
remote repositories.**

## Completion boundary

Topic 35 mein Git identity, distributed model, conceptual areas, content/history overview,
capabilities and limitations complete hue. **Topic 36 — Repository kya hai?** next hai
aur abhi start nahi hua.

