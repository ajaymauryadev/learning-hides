# Topic 47 — `git add`

## Learning goal

`git add` ka staging behavior, exact path selection, content-snapshot timing, deletion handling,
ignored-file boundary aur safe review workflow samajhna hai. Sirf reviewed TaskForge `.gitignore`
ko stage karke real state verify karni hai.

## Simple definition

**`git add` selected path ka current content staging area/index mein copy karta hai, jisse woh next
commit ke proposed snapshot ka part ban sake.**

```text
working-tree content
      | git add <path>
      v
staging area / index
      | later git commit
      v
repository history
```

`git add` commit create ya remote upload nahi karta.

## Practical command

```powershell
git add -- taskforge-backend/.gitignore
```

Command anatomy:

```text
git                              -> Git program
add                              -> index update subcommand
--                               -> options end; next text ko path treat karo
taskforge-backend/.gitignore     -> exact selected path
```

`--` simple filename ke liye mandatory nahi, lekin safe habit hai. Agar filename `-` se start ho,
to Git use option samajhne ke bajay path samjhega.

## Current practical decision

Baseline evidence:

```text
staged paths: 0
taskforge-backend/.gitignore: exists and is not ignored
```

Sirf `.gitignore` select ki gayi because:

- Topic 46 mein content aur ignore decisions verify hue;
- file secret nahi rakhti;
- exact path clearly scoped hai;
- other learning files ko blindly bulk-stage nahi karna tha.

Expected result:

```text
A  taskforge-backend/.gitignore
```

First-column `A` ka meaning new file staging area mein added hai. File abhi commit/history ka part
nahi bani.

## Staging area reminder

Index next commit ka proposed snapshot hai:

```text
HEAD       -> last committed snapshot
index      -> proposed next snapshot
work tree  -> current editable files
```

`git add` work tree se index transition karta hai. It does not necessarily mean “Git permanently
starts watching forever”; it records selected current content in index.

## Content timing: important behavior

Suppose file mein version A hai:

```text
write A -> git add file -> index has A
                      -> edit work tree to B
```

Ab:

```text
index:      A
work tree:  B
status:     AM or MM-like two-state signal, path history par depend
```

Next commit staged A lega, latest B nahi. Latest work-tree content include karne ke liye file ko
dobara review karke `git add` karna padega.

## New, modified and deleted paths

`git add` index ko selected working-tree state se update kar sakta hai:

```text
new file selected      -> addition staged
modified file selected -> modification staged
deleted file selected  -> deletion staged (appropriate pathspec form)
```

Name “add” hone ke baad bhi deletion stage kar sakta hai: actual idea index ko working state se
update karna hai.

## Exact path versus broad pathspec

### Exact file

```powershell
git add -- taskforge-backend/.gitignore
```

Sabse controlled selection.

### Directory

```powershell
git add -- taskforge-backend/
```

Directory ke matching changes recursively stage ho sakte hain. Run se pehle status inspect karo.

### Current directory

```powershell
git add .
```

Current directory ke under broad selection hai. Current working directory badalne par scope badal
jata hai; beginner ke liye blind use risky hai.

### All repository changes

```powershell
git add -A
```

Repository-wide additions, modifications and deletions stage kar sakta hai. Only when full status
reviewed and one coherent commit intended ho.

### Interactive patch

```powershell
git add -p
```

File ke selected hunks stage karne deta hai. Useful jab one file mein multiple logical changes hon.
Prompt decisions carefully read karo; detailed use later practice mein repeat hoga.

## Pathspec kya hai?

Git command ka path argument plain filesystem name ke saath special matching behavior bhi support
kar sakta hai; ise pathspec kehte hain. Wildcards/scope accidentally extra files select kar sakte
hain. Safe beginner workflow mein exact quoted path aur `--` prefer karo:

```powershell
git add -- "path with spaces/file.md"
```

## Ignored files and `git add`

Normal `git add` ignored untracked files skip karta hai. Force option exist karti hai:

```powershell
git add -f -- some-ignored-path
```

Lekin `.env`, credentials, `node_modules` ya generated output ko force-add nahi karna chahiye.
Ignore behavior unexpected ho to Topic 46 ka `git check-ignore -v` use karo, force ko default fix
mat banao.

## Staging is not security approval

Staging reversible preparation layer hai, but staged content accidental commit ke closer hota hai.
Before staging and again before commit:

- secret values inspect karo;
- generated/binary files ka reason verify karo;
- unrelated changes separate rakho;
- staged diff review karo;
- meaningful commit boundary choose karo.

## Status and diff after add

```powershell
git status --short
git diff -- taskforge-backend/.gitignore
git diff --cached -- taskforge-backend/.gitignore
```

Expected relationship after new file stage:

```text
status           -> A  path
ordinary diff    -> no unstaged difference for that path
cached diff      -> staged new-file content
```

Exact diff reading Topics 48–49 mein detail se hoga; abhi transition evidence ke liye use hua.

## Unstage versus discard

```text
unstage -> index selection hatao/change karo, work-tree content preserve karo
discard -> work-tree changes hata sakta hai
```

In dono ko confuse mat karo. Git version/state ke according unstage command differ/context matter
kar sakta hai. Current lesson staged `.gitignore` ko intentionally staged छोड़ता है; use discard
nahi kiya gaya.

## Data/control flow

```text
exact path chosen
  -> Git reads current work-tree content
  -> ignore/pathspec rules evaluated
  -> index entry updated
  -> status reports staged state
```

No commit object, remote request, application runtime or database operation hoti hai.

## Safe execution order

```text
git status --short --branch
  -> target content inspect
  -> secret/generated-file check
  -> git add -- exact/path
  -> git status --short
  -> git diff --cached -- exact/path
  -> only later commit
```

## Common errors aur explanations

### `pathspec ... did not match any files`

Path spelling ya current working directory wrong ho sakti hai, ya path exist/match nahi karta.
Location and status inspect karo; guessing ke saath broad wildcard mat use karo.

### Ignored file add nahi hui

Applicable ignore rule match hui. `git check-ignore -v --no-index <path>` se reason find karo.
Sensitive/generated path ke liye force-add mat karo.

### `git add .` ne extra files stage kar di

Broad scope mein unrelated changes the. Staged list/diff inspect karo, work-tree content discard kiye
bina unwanted index selection safely unstage karo, then exact paths reselect karo.

### Add ke baad latest edit commit mein nahi gaya

`git add` ne us time ka content snapshot index mein rakha. Later edit unstaged hai; review and add
again needed.

### Add ko commit samajhna

Index update local preparation hai. Commit object/history only `git commit` se बनेगी।

### Secret stage ho gaya

Commit se pehle stop karo. Secret ko staged selection se remove, working copy/security need inspect,
ignore rule correct and credential exposure assess karo. Commit/push bilkul mat karo.

## Student exercise

1. `git add` ka main destination kya hai?
2. Add ke baad file edit karoge to latest edit automatically staged hogi?
3. `git add .` exact file add se riskier kyun hai?
4. `--` ka purpose kya hai?
5. Kya staged file committed ho chuki hai?
6. Ignored `.env` ko `-f` se add karna chahiye?

## Exercise answers

1. Staging area/index.
2. Nahi; index mein earlier content रहेगा, later edit unstaged hogi.
3. It can select many matching changes under current directory, including unrelated work.
4. Options end karke following argument ko path treat karne mein help karta hai.
5. Nahi; it is only proposed for next commit.
6. Nahi; secret ko force-stage nahi karna chahiye.

## Interview question with Hinglish answer

**Question:** `git add` kya karta hai?

**Answer:** `git add` selected file ke current content ko staging area ya index mein copy karta hai,
jisse woh next commit ke proposed snapshot ka part ban sake. Yeh commit create nahi karta. Agar file
staging ke baad edit ho, to new edit automatically staged nahi hoti; review karke dobara add karna
padta hai.

## Easy-English minimum interview answer

> `git add` copies the current content of selected files into the staging area. The staging area is
> the proposed snapshot for the next commit. It does not create a commit, and later edits must be
> staged again.

## Completion boundary

Topic 47 mein only reviewed TaskForge `.gitignore` exact path se stage hui. No commit, push, pull,
restore, deletion or application change performed. Topic 48 — `git diff` abhi start nahi hua.
