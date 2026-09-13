# Topic 48 — `git diff`

## Learning goal

Plain `git diff` ka comparison boundary, patch anatomy, path filtering, summary options, untracked
file limitation aur safe review workflow samajhna hai. Current Topic 48 tracker edits ko real
unstaged diff ke roop mein verify karna hai.

## Simple definition

**Plain `git diff` working tree ke tracked content ko staging area/index se compare karta hai aur
unstaged content differences dikhata hai.**

```text
index / staging area
        ↕ plain git diff
working tree
```

Yeh by default `HEAD` versus index comparison nahi hai; woh staged-diff boundary Topic 49 hai.

## Baseline evidence

Topic start hone par repository clean thi:

```text
branch: main
HEAD: a5a3b514a15cc10dd81192baef09ad4a8a63e57f
unstaged diff: empty
staged diff: empty
```

Iska matlab student ne previous learning work commit kar diya tha. Topic 48 ke tracker edits ke baad
tracked documentation files modified hue; wahi real unstaged review material hain.

## Basic command

```powershell
git diff
```

Command anatomy:

```text
git   -> Git program
diff  -> content differences calculate/display subcommand
```

Command normal use mein files modify, stage, commit ya upload nahi karta.

## Three-state comparison map

```text
HEAD snapshot
    ↕ git diff --cached       (Topic 49)
index / staging area
    ↕ git diff                (Topic 48)
working tree
```

Additional explicit comparisons possible hain, but beginner ko pehle comparison endpoints clear
rakhne hain. “Diff empty” ka meaning tabhi समझ आएगा जब पता हो किन दो states को compare किया।

## Current practical comparison

Topic documentation updates ke baad:

```powershell
git diff --name-status
git diff --stat
git diff -- notes/BACKEND_ROADMAP.md
```

Expected tracked unstaged paths:

```text
LEARNING_MEMORY.md
notes/ARCHITECTURE.md
notes/BACKEND_ROADMAP.md
notes/LEARNING_STATE.md
```

New lesson file untracked hai, isliye plain `git diff` uska content नहीं दिखाता।

## Patch anatomy

Typical patch:

```diff
diff --git a/file.md b/file.md
index old..new 100644
--- a/file.md
+++ b/file.md
@@ -10,3 +10,3 @@
-old line
+new line
 unchanged context
```

Meaning:

- `diff --git`: compared path identity/header.
- `index old..new`: abbreviated content object identifiers and mode.
- `--- a/...`: old/index-side label.
- `+++ b/...`: new/working-tree-side label.
- `@@ ... @@`: hunk location header.
- `-`: old side se removed line.
- `+`: new side par added line.
- leading space: unchanged context line.

Header ke `---`/`+++` ko actual deletion/addition lines se confuse nahi karna.

## Hunk kya hota hai?

Hunk nearby related changed lines ka patch section hai. File mein distant edits hon to multiple
hunks हो सकते हैं। Hunk header example:

```text
@@ -oldStart,oldCount +newStart,newCount @@
```

Yeh line positions batata hai; source code ka part nahi hota.

## Exact path filter

```powershell
git diff -- notes/BACKEND_ROADMAP.md
```

`--` options aur pathspec ko separate karta hai. Exact path focus review ko छोटा और clear बनाता है।
Spaces wale path ko quote karo:

```powershell
git diff -- "folder/file name.md"
```

## Useful summary forms

### Changed path and state

```powershell
git diff --name-status
```

Example codes:

```text
M  modified
A  added in compared boundary (plain diff usually tracked/index context dependent)
D  deleted
R  rename presentation when detection applies
```

### Change-size summary

```powershell
git diff --stat
```

Files and insertion/deletion counts summarize karta hai, content review replace नहीं करता।

### Only path names

```powershell
git diff --name-only
```

Changed tracked paths list karta hai.

### Smaller context

```powershell
git diff --unified=2 -- path
```

Hunk ke around two context lines. Default context often easier; too little context meaning hide kar
sakta hai.

### Word-level presentation

```powershell
git diff --word-diff -- path
```

Long prose line ke small word changes inspect karne mein helpful. Code review mein line diff often
clearer ho sakta hai.

## Untracked file limitation

Plain `git diff` normally untracked file content नहीं दिखाता because file ki index entry नहीं है।

```text
new untracked file
  -> git status: ?? path
  -> git diff: usually no content for that file
```

Isliye safe review combination:

```powershell
git status --short
git diff
```

Status untracked paths reveal karta hai; untracked content separately file viewer/editor se inspect
karo. Sirf empty diff dekhkar “no changes” conclude mat karo.

## Staged file limitation

Agar path fully staged hai and staging ke baad edit nahi hua:

```text
index content == working-tree content
plain git diff -> empty for that path
```

Change index versus `HEAD` mein exist kar sakta hai. Uske liye `git diff --cached` Topic 49 mein.

## Whitespace checks

```powershell
git diff --check
```

Common whitespace problems जैसे trailing whitespace report kar sakta hai. Exit code `0` means
checked unstaged patch mein configured whitespace error detect नहीं हुआ। यह syntax test या automated
application test नहीं है।

## Line-ending warning

Windows workspace mein Git warning दे सकता है:

```text
LF will be replaced by CRLF the next time Git touches it
```

Yeh content-change failure necessarily नहीं है। Git configuration और working-tree line-ending
normalization difference बताता है। Blind global config change नहीं करेंगे; repository policy बाद
में intentionally decide होगी। Diff में unexpected every-line change दिखे तो line endings inspect
करना चाहिए।

## Binary files

Git binary content ka useful line patch नहीं दिखा पाता; it may report binary files differ. Large
generated/binary assets ko source history mein रखने का reason verify करो। Attachments future backend
storage design ka part hain, repository mein user uploads commit karna default design नहीं होगा।

## Diff is not history

```text
git diff output -> temporary comparison view
git commit       -> durable repository history object
```

Diff save/record नहीं करती। File edit hone par next diff change ho सकती है।

## Data/control flow

```text
Git reads index entries
  + reads tracked working-tree content
  -> calculates line/content differences
  -> applies optional path filters/output format
  -> prints patch or summary
```

No staging, commit, remote request, TaskForge API call or database operation hoti hai.

## Safe review execution order

```text
git status --short --branch
  -> git diff --name-status
  -> git diff --stat
  -> git diff -- exact/path
  -> git diff --check
  -> inspect untracked files separately
  -> only then decide what to stage
```

## Common errors aur explanations

### `git diff` empty hai but status dirty hai

Changes fully staged ya only untracked ho सकती हैं। `git status --short`, then correct comparison
boundary inspect karo. Topic 49 staged diff cover karega.

### New file diff mein nahi dikh rahi

Untracked file ki index baseline nahi hoti. Status se path identify aur file content separately
review karo; staging only after review.

### Wrong file ka huge diff aa gaya

Broad repository comparison ho raha hai ya generated/line-ending changes hain. `-- exact/path`,
`--stat`, ignore rules aur line endings inspect karo.

### `-` line dekhkar working file delete samajhna

Minus patch line old side se removal batati hai; entire file deletion तभी जब headers/status ऐसा
indicate करें।

### Diff देखकर समझा commit हो गया

Diff current state comparison है, history record नहीं। Commit existence `git log`/status boundary
से verify होगी।

### `git diff --check` pass ko tests pass samajhna

It checks selected whitespace issues, application behavior नहीं। Automated tests separate हैं।

## Student exercise

1. Plain `git diff` किन दो states को compare करता है?
2. Untracked file plain diff में क्यों नहीं दिख सकती?
3. `+` और `-` lines का basic meaning क्या है?
4. Exact file diff command लिखो।
5. `--stat` full content review का replacement है?
6. Empty plain diff क्या clean repository prove करती है?

## Exercise answers

1. Working tree versus staging area/index.
2. उसकी index baseline entry नहीं होती।
3. `-` old side से removed; `+` new side पर added line.
4. `git diff -- path/to/file`.
5. नहीं, only size/path summary है।
6. नहीं; staged या untracked changes हो सकती हैं।

## Interview question with Hinglish answer

**Question:** Plain `git diff` क्या दिखाता है?

**Answer:** Plain `git diff` tracked files के working-tree content को staging area/index से compare
करके unstaged changes दिखाता है। यह normally untracked files या fully staged changes नहीं दिखाता।
मैं इसे `git status` के साथ use करता हूँ और staging से पहले exact content review करता हूँ।

## Easy-English minimum interview answer

> Plain `git diff` shows unstaged changes by comparing tracked working-tree content with the staging
> area. It normally does not show untracked files or fully staged changes. I use it with `git status`
> before staging files.

## Completion boundary

Topic 48 में real unstaged documentation changes inspect और diff quality verify हुई। कोई file stage,
commit, restore, delete या push नहीं हुई। Topic 49 — `git diff --cached` अभी start नहीं हुआ।
