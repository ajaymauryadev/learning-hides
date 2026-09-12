# Topic 41 — Commit

## Learning goal

Git commit ko local history record/snapshot reference ke roop mein samajhna, commit object
ke tree, parent, author, committer, message and hash fields reason karna, aur commit ko
save/stage/push/release se distinguish karna hai. `git commit` command Topic 50 mein.

## Simple definition

**Commit Git repository mein recorded history object hai jo project snapshot/tree ko point
karta hai, parent commit relationship and metadata store karta hai, aur content-derived
identifier se address hota hai.**

```text
Index proposed snapshot
  -> commit operation later
  -> commit object + tree reference + parent(s) + metadata + message
  -> local history advances
```

Commit meaningful checkpoint ho sakta hai, but correctness automatically prove nahi.

## Commit snapshot hai, sirf diff nahi

Beginner UI often “changes in commit” diff show karti hai. Internally conceptual commit
complete project tree snapshot ko reference karta hai:

```text
Commit A -> Tree A
Commit B -> Tree B

Diff A..B = Git compares Tree A and Tree B
```

Diff comparison result hai; commit's core identity snapshot/tree + metadata relationships
se banti hai.

## Current real commit

Current `HEAD`:

```text
Full commit:  1f39efa70bc3f8858cd79b2b5bd0be3917c8c1e8
Short form:   1f39efa
Object type:  commit
Tree:         c01fd3e76326b8b82909179604014efea7f7a845
Parent:       f96332ec3d290053f62125520f1314be37bfccd1
Subject:      docs: complete TaskForge phase 1 environment foundation
Author date:  2026-09-12T13:10:40+05:30
Committer date: same for this commit
```

Personal author email deliberately learning note mein repeat nahi ki gayi.

## Commit object anatomy

Conceptual raw structure:

```text
tree <tree-object-id>
parent <parent-commit-id>
author <identity + timestamp>
committer <identity + timestamp>

commit message
```

Fields:

- **tree:** repository files/directories snapshot root;
- **parent(s):** history ancestry;
- **author:** original change author identity/time;
- **committer:** person/process that created current commit object/time;
- **message:** human explanation;
- **commit ID:** object content/metadata derived hash identity.

## Tree object

Tree Git snapshot directory structure/path-to-object relationships represent karta hai:

```text
commit
  -> root tree
      -> file/blob entries
      -> nested tree entries
```

Current tree ID recorded above. Tree ID same files/content arrangement reuse kar sakti hai
across commits even if commit metadata/parents differ.

## Blob overview

Blob file content represent karta hai, usually filename/path metadata directly blob mein
nahi; tree associates names/modes with object IDs. Git source semantics nahi samajhta:

```text
blob bytes/content
tree path/name relationship
commit history/context
```

Detailed object plumbing advanced Git section mein deepen ho sakta hai.

## Parent relationship

Normal commit commonly previous commit ko parent reference karta hai:

```text
A <- B <- C
```

Current commit `1f39efa` parent `f96332e` ko reference karta hai.

Special cases:

- root/initial commit: zero parent;
- normal commit: usually one parent;
- merge commit: two or more parents possible.

Parent count history structure tell karta hai, code quality nahi.

## Commit graph

```text
A <- B <- C       main line
      \ <- D      branch line
            \ 
             M    merge commit may have multiple parents
```

Branch Topic 42. Commit objects graph nodes; branches movable references.

## Commit hash/ID

Current full ID 40 hexadecimal characters (current SHA-1 object-format repository):

```text
1f39efa70bc3f8858cd79b2b5bd0be3917c8c1e8
```

Short `1f39efa` convenient display, but only unique within relevant repository/context as
long as abbreviation unambiguous.

Hash changes if commit object's tree, parent, author/committer metadata or message changes.
It is identity/integrity mechanism, not encryption or secret protection.

## Commit immutability mental model

Existing commit object edit-in-place nahi hota. Amend/rebase-like history rewriting creates
new commit object(s) with new IDs, then references move:

```text
old commit C
  -> rewrite/amend
  -> new commit C' with different ID
```

Old object temporarily reachable/recoverable ho sakta hai, but shared history rewrite
collaborators disrupt kar sakti hai. Commands later only safe context mein.

## Author versus committer

Often same, but differ ho sakte hain:

```text
Author    -> original patch/change creator
Committer -> current commit object/history application creator
```

Rebase/cherry-pick/patch integration author preserve but committer change kar sakta hai.
Identity metadata verified/trusted signature automatically nahi; signing is separate.

## Commit message

Good message future developer ko intent communicate kare:

```text
<type/scope>: concise outcome

optional body:
why change needed
important tradeoff/behavior
```

Current subject:

```text
docs: complete TaskForge phase 1 environment foundation
```

It states category and outcome better than `changes` or `final-final`.

## What gets committed?

Commit normally current **index/staging snapshot** record karta hai:

```text
HEAD: previous snapshot
Index: proposed snapshot
Working tree: may contain extra unstaged changes

commit -> records Index, not all Working tree automatically
```

Untracked and unstaged changes remain outside commit unless explicitly selected/staged.

## Empty commit possibility

Git options empty commit create allow kar sakte hain even when tree same as parent. It may
serve workflow marker but should require real reason. “No staged change” universally means
commit impossible only without special option; beginner workflow won't create empty commits.

## Commit is local

```text
commit -> local repository history
push   -> local commit(s) remote repository ko transfer
```

Commit success GitHub update proof nahi. Remote may be absent/offline/reject push.

## Commit is not save

```text
Editor save -> working-tree file on disk
Stage       -> index proposed snapshot
Commit      -> index snapshot local history
Push        -> share commits remotely
Deploy      -> application environment update
```

Five separate state transitions.

## Commit is not release/deployment

Commit can trigger CI/deployment automation in configured system, but Git commit itself
does not run production deployment by definition. Pipeline status separate evidence.

## Atomic/coherent commit

Professional commit ideally one understandable purpose:

```text
Feature implementation
  + matching tests
  + relevant docs/API contract
```

Avoid mixing:

```text
auth fix + formatting whole repo + unrelated analytics + secret file
```

Atomic doesn't mean one file; it means coherent behavior/change reason.

## A commit should build/test?

Ideal history commits should be reviewable and reasonably valid according to project
Definition of Done. During local experimentation temporary commits may exist, but shared
history quality matters. Tests relevant to staged snapshot, not only combined working tree.

## Commit metadata privacy

Author name/email and message become repository history and may be published on remote.
Use intended professional identity/privacy settings. Do not put passwords, tokens, customer
data or sensitive incident details in commit message.

## Secret in commit

Deleting secret in next commit does not remove it from old commit:

```text
Commit A -> secret added
Commit B -> secret deleted

History A still contains secret
```

Rotate credential immediately and perform approved history remediation when necessary.
Prevention Topic 56.

## Verification commands used

Read-only:

```powershell
git rev-parse HEAD
git rev-parse --short HEAD
git cat-file -t HEAD
git rev-parse "HEAD^{tree}"
git show -s --format="..." HEAD
git cat-file -p HEAD
```

Raw output can include personal identity; share/log only needed safe fields.

## Current working state versus HEAD

Current `HEAD` remains Phase 1 commit. Phase 2 Topics 34–41 working changes are not part of
it:

```text
HEAD commit: Phase 1 snapshot
Working tree: Phase 2 docs in progress
Index: no staged differences at topic start
```

Topic 41 did not create a new commit.

## Common mistakes aur fixes

### Commit = diff

Commit references snapshot tree; diff is comparison between states.

### Commit records every working change

It records index snapshot. Review staged and unstaged separately.

### Commit = push

Commit local; push remote operation.

### Save = commit

Save disk state only.

### Hash encrypts secret

History content retrievable; hash identity is not encryption.

### Change old commit in place

Rewrite creates new IDs and moves references; shared-history risk.

### Short hash globally unique

It is repository/context abbreviation and can become ambiguous.

### Good message = correct code

Tests/review/security evidence still required.

### Delete secret next commit solves leak

Old commit remains; rotate/remediate.

### Author equals committer always

They often match but can differ in transformed/integrated history.

## Safe commit preparation preview

```text
1. repository/branch/status verify
2. working and untracked content inspect
3. run relevant tests/checks
4. stage exact coherent content
5. inspect staged names and staged diff
6. scan for secrets/generated/unrelated files
7. write outcome/why message
8. create commit later
9. inspect commit and remaining working tree
10. push only after intended remote/branch checks
```

Exact commands Topics 45–53.

## TaskForge commit design

Examples of coherent future commits:

```text
feat(auth): add validated user registration
test(auth): cover duplicate email rejection
docs(api): document registration contract
```

Often feature + tests + docs may belong in one atomic commit if one behavior. Split strategy
review/revert needs par.

## Student exercise

1. Commit one sentence define.
2. Current commit tree/parent/subject identify.
3. Snapshot vs diff explain.
4. Author vs committer.
5. Save/stage/commit/push/deploy sequence.
6. Root/normal/merge parent counts.
7. Why amend creates new ID?
8. Why deleting secret later insufficient?

## Exercise answer

```text
commit = local history object referencing snapshot tree, parents and metadata
tree=c01fd3e..., parent=f96332e..., subject=Phase 1 completion
snapshot is state; diff compares two states
author wrote original change; committer created/applied commit object
save -> stage -> commit -> push -> deploy (separate operations)
root=0, normal usually=1, merge=2+
commit object content/metadata changes, so content-derived ID changes
old commit still contains deleted secret; rotate and remediate history
```

## Interview question with Hinglish answer

**Question:** Git commit kya hota hai aur usmein kya information hoti hai?

**Answer:** Commit local repository ka history object hai jo project tree snapshot ko
reference karta hai, parent commit(s), author, committer, timestamps and message contain
karta hai, and hash ID se identify hota hai. Commit staged index snapshot record karta hai,
all unstaged/untracked working files automatically nahi. Commit local hai; push and deploy
separate actions hain. Rewrite/amend new commit ID create karta hai.

## Easy-English minimum interview answer

**A Git commit is a local history object that references a project snapshot, its parent
commit, author and committer metadata, and a message. It records the staged snapshot, not
every current working-tree change.**

Short version:

**A commit records the staged project snapshot in local Git history and links it to earlier
history.**

## Completion boundary

Topic 41 mein commit object, snapshot/tree, parent graph, metadata/hash, local boundary and
safety complete hue. **Topic 42 — Branch** next hai aur abhi start nahi hua.

