# Topic 42 — Branch

## Learning goal

Git branch ko lightweight movable commit reference ke roop mein samajhna, `HEAD`
attachment, branch movement/divergence, working changes, integration and safe branch
operations reason karna hai. Remote repository Topic 43 mein.

## Simple definition

**Git branch ek named, movable reference hoti hai jo commit history ke kisi commit ko
point karti hai and new commits ke saath usually forward move karti hai.**

```text
A <- B <- C
          ↑
         main
```

Branch poore project folder ki duplicate copy nahi; commit graph position ka human-readable
name/reference hai.

## Current real evidence

```text
Current branch: main
Symbolic HEAD: main
Full HEAD ref: refs/heads/main
HEAD commit: 1f39efa70bc3f8858cd79b2b5bd0be3917c8c1e8
main commit: 1f39efa70bc3f8858cd79b2b5bd0be3917c8c1e8
Local branch count: 1
```

Graph now:

```text
a8ab039 <- 4fd99d8 <- f96332e <- 1f39efa
                                      ↑
                                 HEAD -> main
```

## `HEAD` kya hai?

`HEAD` current checkout/reference context identify karta hai. Normal attached state:

```text
HEAD -> refs/heads/main -> commit 1f39efa
```

Read-only commands:

```powershell
git branch --show-current
git symbolic-ref --short HEAD
git symbolic-ref HEAD
git rev-parse HEAD
git rev-parse main
```

Current `HEAD` attached to local `main` branch hai.

## Branch name and full ref

Human branch name:

```text
main
```

Full local reference:

```text
refs/heads/main
```

Git reference namespaces distinguish local branches, tags and remote-tracking references.
Short name convenient; full ref removes ambiguity.

## Branch pointer kaise move karta hai?

Conceptual new commit on current branch:

```text
Before:
A <- B
     ↑
HEAD -> main

Commit C:
A <- B <- C
          ↑
     HEAD -> main
```

Old commit B mutate nahi; new C parent B ko point karta and `main` reference C par move
hoti hai.

## Branch create karna conceptually

Existing commit B se new name:

```text
A <- B
     ↑  ↑
   main feature/tasks
```

Creating branch by itself usually files/history duplicate nahi; one new ref. Switching
working tree checkout state affect kar sakta hai. Topic 42 ne create/switch command run
nahi ki.

## Divergence

Different branches par commits:

```text
        C <- D  feature/tasks
       /
A <- B <- E    main
```

Both histories common ancestor B share karti hain, then diverge.

## Branch ka purpose

- feature work isolate;
- bug fix line;
- experiment without moving main ref;
- review/pull-request workflow;
- release/hotfix policy;
- parallel team development.

Branch isolation perfect runtime/environment isolation nahi. Same database, secrets, ports
or generated files still shared ho sakte hain.

## Branch is not a folder copy

Incorrect beginner mental model:

```text
main/ folder + feature/ folder always created
```

Normal single working tree one checked-out snapshot at a time materialize karta hai.
Linked worktrees can give multiple directories, but branch concept itself directory copy
nahi.

## Working tree and branch switching

Current uncommitted changes commit se attached history record nahi. Branch switch par Git
may:

- safely carry compatible working changes;
- refuse switch if changes would be overwritten;
- cause different files to materialize;
- leave ignored/untracked files unless collision occurs.

Therefore “changes current branch ke hain” only after commit association; unstaged changes
working-tree state hain.

## Current important safety condition

Current working tree has Phase 2 uncommitted documentation. Branch switching/checkout could
carry or conflict with it. Topic 42 did not switch because:

```text
inspect dirty state -> preserve user work -> learn concept only
```

Never use checkout/reset to discard unknown changes.

## Detached HEAD

Detached HEAD means `HEAD` directly commit ko point karta, local branch name ko nahi:

```text
HEAD -> commit B
main -> commit C
```

You can inspect/build and even commit, but new commits may lack durable branch name if you
move away. Preserve needed work by intentional branch/ref before it becomes unreachable.

`git branch --show-current` detached state mein empty output de sakta hai. Empty output
command failure automatically nahi; symbolic-ref check context clarify karta hai.

## Merge overview

Merge histories integrate karta hai.

Fast-forward possibility:

```text
A <- B <- C feature
     ↑ main

main simply moves to C if no divergence
```

Merge commit possibility:

```text
        C feature
       / \
A <- B   M main
       \ /
        D main-before-merge
```

Merge commit multiple parents have sakta hai. Exact workflows later.

## Rebase overview

Rebase commits ko new base par replay karke new commit IDs create karta hai:

```text
original: B <- C
new base: B <- D <- C'
```

History rewrite shared commits/collaboration affect kar sakti hai. Without understanding,
published history rebase/force-push nahi.

## Merge conflict

When Git cannot safely combine changes automatically:

```text
same base/area changed differently
  -> conflict markers/index conflict stages
  -> human understands both intents
  -> correct result edits
  -> tests and staged diff verify
```

Conflict means business logic wrong automatically nahi, and “ours/theirs” blind choice can
lose valid work.

## Branch naming

Names should communicate purpose:

```text
feature/task-assignment
fix/duplicate-email-validation
docs/api-auth-contract
```

Avoid vague `new`, `test2`, `final`. Team policy may impose prefixes/ticket IDs. Do not put
secrets/customer PII in branch names because remotes/logs/UI expose them.

## Main/default branch

Current branch named `main`. Default branch name repository/hosting configuration chooses;
not universally `main` or `master`. Scripts/CI should use actual configured branch/policy,
not hard-code assumption without need.

## Local branch versus remote-tracking branch

Preview:

```text
main          -> local branch
origin/main   -> local reference representing last known remote state
remote main   -> branch in remote repository
```

They can point different commits. `origin/main` live network view nahi; fetch updates it.
Remote detail Topic 43.

## Branch ahead/behind

Relative commit reachability:

```text
local has commits remote-tracking lacks -> ahead
remote-tracking has commits local lacks -> behind
both have unique commits -> diverged
```

Counts depend on last fetch knowledge; stale refs can produce stale view.

## Branch deletion

Deleting branch deletes named reference, not necessarily commit objects immediately. If
commits reachable elsewhere, safe; unique unmerged commits can become harder to find and
eventually pruned. Before delete:

```text
branch target -> merge/reachability -> remote status -> valuable commits -> recovery plan
```

Topic 42 performed no deletion.

## Branch rename

Renaming local branch changes reference name. Remote upstream, CI, links and teammates may
still reference old name. Rename is multi-system coordination, not only cosmetic.

## Branch protection

Hosting platforms can require reviews/status checks and restrict force pushes/deletion on
important branches. Local Git branch itself does not enforce remote platform policy. Exact
GitHub rules current project need par.

## Feature branch tradeoff

Benefits:

- isolation and review;
- parallel work;
- clean main line policy.

Costs:

- long-lived divergence;
- merge conflicts;
- integration delay;
- stale dependencies/design.

Prefer small short-lived coherent branches when workflow supports, but team context matters.

## Read-only commands used

```powershell
git branch --show-current
git branch --list
git symbolic-ref --short HEAD
git symbolic-ref HEAD
git rev-parse HEAD
git rev-parse main
git show-ref --heads
```

No create/switch/merge/rebase/delete/rename operation.

## Current branch evidence flow

```text
HEAD symbolic ref = refs/heads/main
  -> main ref resolves to 1f39efa...
  -> HEAD resolves to same commit
  -> attached current branch confirmed
```

## Common mistakes aur fixes

### Branch is full project copy

It is movable commit reference; working tree materializes selected state.

### New files automatically belong to branch

Uncommitted working changes may move across switch; commit associates snapshot with branch
history position.

### Commit automatically updates every branch

Only current ref normally advances; other refs remain.

### Local `main` equals remote main always

Remote-tracking knowledge can be stale/diverged. Fetch/status evidence.

### Branch switching safely hides all changes

Git may carry/refuse due dirty working tree. Inspect and preserve.

### Detached HEAD means repository broken

Valid inspection state, but new commits need durable ref planning.

### Delete branch deletes all files/history immediately

Reference deletion and object reachability differ; unique commits risk.

### Rebase moves same commits

Replay creates new commit IDs; shared-history consequences.

### Conflict resolve by choosing one side blindly

Understand intent, integrate, test and diff review.

## Safe branch-operation checklist

```text
1. repository top-level verify
2. current branch and HEAD inspect
3. working/index/untracked state inspect
4. preserve/commit/stash only with understood workflow
5. target branch/ref verify
6. switch/create exact intended name
7. status/files/tests verify after switch
8. integration diff/commits/conflicts review
9. remote/upstream policy inspect before publish/delete/rewrite
```

Stash command later only when real need and understood recovery; not automatic cleanup.

## TaskForge branch workflow preview

```text
main -> stable reviewed baseline
  \
   feature/task-crud -> implementation + tests + docs
                      -> review/integrate
```

Current curriculum changes remain on `main` working tree because user workflow has not
requested branch creation. We will not invent branch policy before repository boundary and
remote decisions.

## Student exercise

1. Branch one sentence define.
2. Draw `HEAD -> main -> commit`.
3. Current branch/full ref/commit state.
4. Branch vs folder copy.
5. Dirty changes and switching risk.
6. Detached HEAD.
7. Fast-forward vs merge commit overview.
8. Why rebase changes IDs?
9. Local branch vs remote-tracking branch.

## Exercise answer

```text
branch = named movable commit reference
HEAD -> refs/heads/main -> 1f39efa...
current main; one local branch; attached HEAD
branch does not duplicate whole folder
dirty changes may carry or block/overlap switch
detached HEAD points directly to commit
fast-forward moves ref; merge commit joins diverged parents
rebase creates replayed commit objects with new parents/IDs
main local; origin/main last-known remote reference
```

## Interview question with Hinglish answer

**Question:** Git branch kya hoti hai aur `HEAD` ka kya role hai?

**Answer:** Branch named movable reference hai jo commit ko point karti hai; new commit par
current branch normally forward move karti hai. `HEAD` current checkout ko identify karta
hai—normal attached state mein `HEAD` local branch ref ko point karta hai. Branch folder
copy nahi. Switch se pehle dirty working tree inspect karta hoon because uncommitted changes
carry/block ho sakti hain. Detached HEAD direct commit ko point karta hai.

## Easy-English minimum interview answer

**A Git branch is a named, movable reference to a commit. HEAD identifies the current
checkout and normally points to the active local branch. Creating a branch does not copy
the whole project directory.**

Short version:

**A branch is a movable name for a line of commits, and HEAD shows the current checkout.**

## Completion boundary

Topic 42 mein branch/ref/HEAD, movement, divergence, integration, detached state and safety
complete hue. **Topic 43 — Remote repository** next hai aur abhi start nahi hua.

