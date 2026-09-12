# Topic 38 — Untracked file

## Learning goal

Untracked file ka exact Git meaning, status evidence, tracked/modified/ignored difference,
lifecycle, backup/recovery limitation and safe handling samajhna hai. `git add` Topic 47
mein practical hoga; aaj staging/commit/delete nahi.

## Simple definition

**Untracked file working tree mein exist karti hai, lekin Git ke index/current tracked
history ko abhi us path ka content known nahi hota.**

```text
File exists on disk
  + Git tracking set/index mein path absent
  = untracked file
```

Git automatically har created file ko version history mein add nahi karta.

## Current real evidence

Topic-start command:

```powershell
git ls-files --others --exclude-standard
```

Returned four untracked files:

```text
notes/topics/phase-2/034-version-control.md
notes/topics/phase-2/035-what-is-git.md
notes/topics/phase-2/036-what-is-a-repository.md
notes/topics/phase-2/037-working-tree.md
```

Topic 38 note create hone ke baad final count five hoga. These valuable learning files
hain—untracked ka meaning temporary/disposable nahi.

## `??` status marker

```powershell
git status --short
```

Current compact output includes:

```text
?? notes/topics/phase-2/
```

`??` means Git sees untracked content. Directory collapsed form entire folder show kar
sakti hai rather than each child. Detailed status parsing Topic 45.

## Directory collapsed kyun?

Git status performance/readability ke liye untracked directory ko one entry mein summarize
kar sakta hai:

```text
?? notes/topics/phase-2/
```

Actual contents inspect:

```powershell
git ls-files --others --exclude-standard
```

Or status option conceptually all files show kar sakti hai:

```powershell
git status --short --untracked-files=all
```

One directory line = one untracked file assume nahi.

## Negative tracked-file verification

```powershell
git ls-files --error-unmatch "notes/topics/phase-2/034-version-control.md"
```

Result:

```text
pathspec did not match any file(s) known to git
exit code: 1
```

This was expected negative probe: file exists but Git tracked-file list ko known nahi.
Expected non-zero result documented check ko failure nahi banata; expectation matters.

## Untracked versus tracked-modified

```text
Untracked:
  disk par new path
  Git baseline/index ko path known nahi

Tracked modified:
  Git path ko already knows
  working content recorded/index state se differs
```

Current examples:

```text
Untracked: notes/topics/phase-2/034-version-control.md
Modified tracked: notes/BACKEND_ROADMAP.md
```

Tracked file Topic 39 mein formalize hoga.

## Untracked versus ignored

Ignored file usually untracked ho sakti hai, but ignore rule normal status/listing se omit
karne ke liye tells Git:

```text
Untracked visible -> not tracked and not excluded by effective ignore rules
Ignored           -> not tracked and matched by ignore rule (normally omitted)
```

Current inspection:

```text
Visible untracked count: 4 at topic start
Ignored untracked count: 0
```

Existing global-ignore permission warning means global layer access incomplete ho sakti
hai; repository evidence ko warning context ke saath read karo.

## Untracked versus unstaged

“Unstaged” broad conversational term working-tree content not selected in index ke liye
use ho sakta hai. Both modified tracked and untracked files unstaged ho sakti hain, but
Git diff behavior differs:

- normal `git diff` tracked modifications show karta hai;
- untracked file content normal `git diff` mein automatically show nahi hota;
- status/untracked-list command se discover karna padta hai;
- stage ke baad staged diff content show kar sakta hai.

Therefore `git diff` empty hona “no new files” proof nahi.

## Untracked lifecycle

```text
new file created
  -> untracked
  -> choose one:
      A. intended project file -> inspect -> stage later -> tracked/staged
      B. generated/private file -> suitable ignore rule later
      C. accidental/temp file -> verify ownership -> safely remove
```

Decision filename alone se nahi; responsibility/content/security inspect karke.

## When file becomes tracked

New path ko staging/index mein intentionally add karne par Git us path ko tracked state
mein lana start karta hai; commit permanent history record later creates. Exact `git add`
Topic 47.

```text
untracked -> stage selected content -> tracked/staged -> commit -> tracked history
```

Staging and committing separate operations hain.

## When tracked file can become untracked

Path ko Git index se remove while disk copy retain karne jaisa workflow possible hai,
but careless use history/tracking expectations change karta hai. Secrets accidentally
tracked hone par simply untrack/ignore enough nahi if history already contains secret;
rotation required. Exact safe workflow Topics 46/56.

## Recovery limitation

Git repository generally untracked file content ka committed backup/history nahi rakhti.
If deleted before staged/committed or separately backed up, Git may not recover it.

```text
untracked + deleted
  -> Git commit history has no content snapshot
  -> recovery not guaranteed
```

This is why `git clean`, shell deletion, reset-like assumptions dangerous hain.

## `git clean` risk

`git clean` untracked files remove karne wala destructive Git command ho sakta hai.

Preview concept:

```powershell
git clean -n
git clean -nd
```

`-n` dry run/preview; `-d` directories include scope. Actual deletion flags/behavior
samjhe bina execute nahi. Preview itself expected targets carefully inspect karo; ignored
items inclusion has separate dangerous options.

Current topic ne `git clean` execute/preview bhi nahi kiya because untracked notes known
valuable work hain.

## Untracked secret risk

Untracked secret remote history mein abhi nahi, but still risks:

- accidental `git add .`;
- filesystem/cloud backup exposure;
- screenshots/log output;
- editor/search plugins;
- future broad commit.

Correct action: secret type identify, ignore policy before staging, safe environment
storage. If already exposed elsewhere, rotate.

## Broad add risk

```powershell
git add .
```

Can select many visible untracked/modified paths under scope. Topic 47 tak run nahi.
Before broad staging:

```text
status -> list every untracked path -> inspect contents -> secret/generated check
-> stage intentional paths -> staged diff review
```

Specific paths beginner safety/review easier bana sakte hain.

## Empty directory nuance

`taskforge-backend/` exists but empty, and `git ls-files --others` no entry shows because
there is no file path/content to track. Directory existence `Test-Path` se verify.

```text
Empty directory != untracked file
```

Once justified file created inside, Git may report that file/directory prefix as untracked.

## File save and untracked state

New file editor buffer mein unsaved ho to filesystem/Git see nahi kar sakta. Saving creates
disk file, then it may appear untracked. Git tracking editor tab creation se nahi, disk
and index state se determined.

## Rename/copy nuance

Tracked file ko outside-Git filesystem command se copy karna new destination ko untracked
bana sakta hai; original stays tracked. Move may show tracked source deleted + new path
untracked until Git evaluates/staging later. Status/diff inspect.

## Submodule/nested repository nuance

Untracked directory ke andar `.git` boundary ho to Git may treat/report differently and
parent may not list inner contents normally. Current Phase 2 folder has no nested `.git`.
Boundary first inspect.

## Safe untracked-file review

```text
1. Git top-level and CWD verify
2. status/untracked list capture
3. collapsed directories expand/list
4. exact file type/name/content inspect safely
5. secrets/generated/temp/project responsibility classify
6. choose track, ignore or remove
7. remove only with ownership/recovery confidence
8. stage later with exact scope
9. staged diff review before commit
```

## Common mistakes aur fixes

### Untracked means ignored

Ignored requires matching effective rule; verify separately.

### Untracked means disposable

Could be valuable new source/docs. Inspect content and ownership.

### `git diff` empty means no changes

Check status/untracked list; normal diff may omit untracked content.

### Folder status line means one file

Expand with untracked listing/all option.

### Delete then recover from Git

No committed snapshot means recovery unavailable. Backup/inspect before delete.

### `git add .` safely tracks everything

May include secrets/logs/generated/unrelated files. Explicit review/staging.

### `.gitignore` after accidental secret commit solves exposure

Ignore affects future untracked consideration, not existing history. Rotate/remediate.

### Negative command exit treated as unexpected failure

Expected `--error-unmatch` exit 1 is valid evidence when checking “not tracked.” Define
expected outcome before command.

## Current TaskForge evidence

At Topic 38 completion expected visible untracked Phase 2 notes:

```text
034-version-control.md
035-what-is-git.md
036-what-is-a-repository.md
037-working-tree.md
038-untracked-file.md
```

They should be tracked eventually through reviewed Git workflow, but Topic 38 only teaches
state. No stage/commit yet.

## Student exercise

1. Untracked file define.
2. Current `??` directory line expand to exact files.
3. Untracked vs modified tracked.
4. Untracked vs ignored.
5. Why normal `git diff` may omit new file content?
6. Three possible decisions: track/ignore/remove with examples.
7. Why `git clean` dangerous?
8. Why untracked secret still risky?

## Exercise answer

```text
untracked = disk path Git index/history does not know
use git ls-files --others --exclude-standard to list exact files
modified tracked has known baseline; untracked has none
ignored matches exclusion rule; visible untracked does not
normal diff compares tracked states, not raw new-file content
track source/docs; ignore generated/secret-local config; remove verified temp
git clean can permanently delete content Git cannot recover
secret may be broadly staged or exposed outside Git
```

## Interview question with Hinglish answer

**Question:** Git mein untracked file kya hoti hai aur safely kaise handle karte ho?

**Answer:** Untracked file working tree mein exist karti hai but Git index/history ko path
known nahi hota. `git status` mein `??` and `git ls-files --others --exclude-standard` se
inspect karta hoon. Content/responsibility check karke intended project file ko later stage,
generated/private file ko ignore, ya verified temp file remove karta hoon. `git diff` alone
untracked content show nahi karta and `git clean` deletion recoverable guarantee nahi.

## Easy-English minimum interview answer

**An untracked file exists in the working tree but is not known to Git's index or history.
I inspect it before deciding to track, ignore, or remove it, because Git may not be able to
recover an untracked file after deletion.**

Short version:

**An untracked file exists on disk but has not yet been added to Git's tracked set.**

## Completion boundary

Topic 38 mein untracked identity, evidence, lifecycle, ignore/diff/recovery boundaries and
safety complete hue. **Topic 39 — Tracked file** next hai aur abhi start nahi hua.

