# Topic 51 — `git log`

## Learning goal

Simple examples se Git history read karna hai: newest commit, commit ID, message, parent, branch
labels, graph, file history aur useful filters. History ko inspect karna hai, change नहीं।

## Sabse simple definition

**`git log` repository ki commit history dikhata hai.**

```text
latest commit
    ↓ parent
older commit
    ↓ parent
more older commit
```

Yeh पूछने का command है:

> “Project mein pehle kaunse save points/commits bane the?”

## Diary wala easy example

Imagine ek project diary:

```text
13 Sep -> Git commit lesson complete
12 Sep -> Git diff lesson complete
11 Sep -> environment setup complete
```

Har diary entry ke paas ID, author, time aur message hai. `git log` diary entries newest-first
dikhata hai. Git history actually parent-linked commit graph hai, सिर्फ text diary नहीं।

## Basic command

```powershell
git log
```

Default output में generally यह fields दिखते हैं:

```text
commit <full hash>
Author: <name and email>
Date:   <date>

    <commit message>
```

Output लंबा हो तो pager खुल सकता है:

```text
Space / PageDown -> आगे
b                -> पीछे (pager support पर depend)
q                -> बाहर
```

`q` Git history delete नहीं करता; सिर्फ viewer बंद करता है।

## Most useful beginner command

```powershell
git log --oneline
```

Current history sample:

```text
fdf6d79 docs: complete phase 2 topics 48 to 50
a5a3b51 docs: complete phase 2 topics 34 to 47
1f39efa docs: complete TaskForge phase 1 environment foundation
```

Each line:

```text
fdf6d79  -> short commit hash/ID
docs...  -> commit subject/message
```

Newest reachable commit ऊपर आता है।

## Full hash versus short hash

Latest commit evidence:

```text
full:  fdf6d797acc1d322b382a82276be353416c2d8ed
short: fdf6d79
```

- Full hash exact object identity है।
- Short hash convenient prefix है, जब repository में unique हो।
- Repository बड़ी होने पर अधिक characters needed हो सकते हैं।

## Latest one commit

```powershell
git log -1
```

Compact form:

```powershell
git log -1 --oneline
```

`-1` means सिर्फ one latest reachable commit दिखाओ। `-5` latest five दिखा सकता है।

## Decorations: branch labels samajhna

```powershell
git log --oneline --decorate -5
```

Observed labels:

```text
fdf6d79 (HEAD -> main) ...
a5a3b51 (origin/main, origin/HEAD) ...
```

Easy meaning:

```text
HEAD -> main -> fdf6d79
cached origin/main -> a5a3b51
```

Local `main` one commit आगे है। `origin/main` local cached remote-tracking ref है, live GitHub query
नहीं।

## Graph view

```powershell
git log --graph --oneline --decorate --all
```

Current history linear है:

```text
* fdf6d79 (HEAD -> main) newest local commit
* a5a3b51 (origin/main) previous commit
* 1f39efa older commit
```

`*` commit node है। Straight line simple parent chain दिखाती है। Branch/merge history में lines
split/join हो सकती हैं।

## `--all` ka meaning

By default log current checked-out history से reachable commits दिखाता है। `--all` repository के
all known refs से reachable history include कर सकता है:

```powershell
git log --all --oneline --decorate
```

Important: “all” का मतलब remote server की unknown live commits नहीं; केवल local repository को
known refs/objects हैं।

## Parent commit

Latest commit ka parent:

```powershell
git log -1 --format=%P
```

Current latest parent `a5a3b51...` है:

```text
a5a3b51 <- parent of - fdf6d79
```

- First/root commit का parent नहीं होता।
- Normal commit का one parent होता है।
- Merge commit के multiple parents हो सकते हैं।

## Custom format

```powershell
git log -1 --format="%h | %an | %ad | %s"
```

Common placeholders:

```text
%H -> full hash
%h -> short hash
%an -> author name
%ae -> author email
%ad -> author date display
%P -> parent full hashes
%s -> subject
```

Only needed fields choose करके output easy बनाया जा सकता है। Email output share करते समय privacy
का ध्यान रखें।

## Author date versus commit date

Git commit में author और committer metadata अलग हो सकती है:

```text
author date    -> original change कब authored हुआ
committer date -> this commit record/rewrite कब बनाया गया
```

Normal simple commit में same हो सकती हैं। Rebase, cherry-pick या patches के बाद differ कर सकती
हैं। “Date” देखते समय format/meaning clear रखें।

## File history

```powershell
git log --oneline -- notes/topics/phase-2/050-git-commit.md
```

यह उस path को affect करने वाले reachable commits दिखाता है। Current result:

```text
fdf6d79 docs: complete phase 2 topics 48 to 50
```

Rename follow करने का option:

```powershell
git log --follow --oneline -- path/to/file
```

`--follow` single-file history में rename detection help कर सकता है, but perfect universal history
reconstruction guarantee नहीं।

## Useful search filters

### Message search

```powershell
git log --oneline --grep="phase 2"
```

Commit message/subject matching history खोजता है। Code content search नहीं।

### Author filter

```powershell
git log --oneline --author="AJAY"
```

Matching author metadata वाले commits दिखाता है।

### Date filter

```powershell
git log --since="2026-09-01" --until="2026-09-30" --oneline
```

Date range filter करता है। Timezone/date semantics ध्यान से interpret करें।

### Path filter

```powershell
git log --oneline -- notes/BACKEND_ROADMAP.md
```

`--` के बाद path है। Path-specific history debugging में useful है।

## Commit summary versus exact content

```text
git log  -> commits list/history/navigation
git show -> one commit ka metadata + patch/content (later repeated use)
```

Example:

```powershell
git show --stat --oneline fdf6d79
```

Hash untrusted source से मिला हो तो verify करें; command read-only हो सकती है, but terminal arguments
blindly paste नहीं करने चाहिए।

## Current history evidence

Topic start पर:

```text
known commits reachable from HEAD: 6
latest: fdf6d79
latest subject: docs: complete phase 2 topics 48 to 50
latest parent: a5a3b51...
local main versus cached origin/main: ahead 1
```

Topic 51 documentation अभी commit नहीं हुई, इसलिए current log में naturally नहीं दिखती। File का
disk पर होना और committed history में होना अलग states हैं।

## Data/control flow

```text
start ref (usually HEAD)
  -> Git reads commit object
  -> follows parent reference(s)
  -> applies filters/path/date options
  -> formats newest-to-older output
```

No working file edit, staging, new commit, remote fetch या database operation होती है।

## Safe history-reading workflow

```text
git status --short --branch
  -> git log --oneline --decorate -10
  -> git log --graph --oneline --decorate --all
  -> exact hash identify
  -> git show --stat <hash>
  -> path-specific log when debugging
```

## Common errors aur easy fixes

### `fatal: your current branch ... does not have any commits yet`

Fresh repository unborn branch पर है। First commit अभी बना नहीं। `git log` empty history create
नहीं करता।

### Output screen में फँस गया

Pager खुला है। `q` press करके बाहर आओ। यह process/history delete नहीं करता।

### Latest local commit GitHub पर नहीं दिख रही

Log local history दिखाता है; commit push नहीं हुई हो सकती। Current local branch cached remote से
ahead हो सकती है।

### `--all` लगाया फिर भी remote की new commit नहीं दिखी

`--all` all locally known refs है। Live server updates के लिए network fetch concept अलग है।

### Wrong commit choose हुआ

Short hashes/messages similar हो सकते हैं। `git log --decorate`, full hash और `git show --stat`
से verify करो।

### File history rename से पहले गायब दिखती है

Single path filter old name नहीं खोज रहा। `--follow` try करो और rename/history limits समझो।

### Log order को development order मान लिया

Default presentation usually commit-date/topology rules से प्रभावित हो सकती है; branches/merges में
simple chronological story assume मत करो। Graph और parents inspect करो।

## Student exercise

1. `git log` क्या दिखाता है?
2. Latest three commits का easy command क्या है?
3. `HEAD -> main` का meaning क्या है?
4. `--all` live remote query करता है?
5. Particular file history कैसे देखोगे?
6. Pager बंद करने की key क्या है?

## Exercise answers

1. Reachable commit history और metadata.
2. `git log --oneline -3`.
3. HEAD current branch `main` को refer करता है, जो shown commit पर point करती है।
4. नहीं, only locally known refs/history.
5. `git log --oneline -- path/to/file`.
6. `q`.

## Interview question with Hinglish answer

**Question:** `git log` का use क्या है?

**Answer:** `git log` repository की commit history inspect करने के लिए use होता है। इससे commit hash,
author, date, message, parent relationship और branch labels देख सकते हैं। मैं `--oneline`,
`--decorate`, `--graph`, filters और path option से debugging/history navigation आसान बनाता हूँ।

## Easy-English minimum interview answer

> `git log` displays the commit history of a repository. It can show commit hashes, authors, dates,
> messages, parents, and branch labels. I use one-line, graph, filter, and path options to find and
> understand relevant commits.

## Completion boundary

Topic 51 में current history multiple read-only formats से verify हुई। कोई staging, commit, push,
fetch, reset, amend या file deletion नहीं हुई। Topic 52 — `git remote` अभी start नहीं हुआ।
