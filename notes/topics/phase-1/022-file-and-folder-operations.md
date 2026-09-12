# Topic 22 — File aur folder operations

## Learning goal

Aaj PowerShell se files/directories ko inspect, create, copy, rename, move aur delete
karna samajhna hai. Focus sirf command yaad karna nahi; target, state change, collision,
verification aur safe cleanup reason karna hai.

Topic verification ek isolated `.topic22-practice` sandbox mein hui. Existing project
files modify nahi hue aur sandbox successful verification ke baad remove ho gaya.

## Operation aur state ka mental model

Filesystem operation current state ko read ya change karti hai:

```text
Before state
  -> command + exact target
  -> filesystem operation
  -> success or error
  -> after state verify
```

Do broad categories:

- **read-only operation:** state inspect karti hai, intentionally change nahi;
- **state-changing operation:** file/directory create, copy, move, rename, edit ya
  delete karti hai.

## File aur directory

- **File** named data/container hoti hai, jaise `server.js` or `package.json`.
- **Directory/folder** files aur nested directories organize karti hai, jaise `src/`.

PowerShell items ko provider objects ki tarah represent karta hai. Filesystem mein
file aur directory dono items hain.

## 1. Inspect/list — `Get-ChildItem`

```powershell
Get-ChildItem -LiteralPath ".\notes"
```

Directory ke direct child items list karta hai.

```powershell
Get-ChildItem -LiteralPath ".\notes" -Recurse
```

`-Recurse` nested descendants tak inspection broad karta hai. Large tree mein output
bahut bada ho sakta hai; target pehle verify karo.

Useful switches:

```powershell
Get-ChildItem -LiteralPath ".\notes" -File
Get-ChildItem -LiteralPath ".\notes" -Directory
```

First only files, second only directories return karta hai.

## 2. Existence/type check — `Test-Path`

```powershell
Test-Path -LiteralPath ".\notes" -PathType Container
Test-Path -LiteralPath ".\notes\BACKEND_ROADMAP.md" -PathType Leaf
```

```text
Container = directory-like target
Leaf      = file-like target
```

`True` existence/type match prove karta hai. File content correct hai, yeh prove nahi
karta.

## 3. Directory create — `New-Item`

```powershell
New-Item -ItemType Directory -Path ".\example"
```

Anatomy:

```text
New-Item           = create command
-ItemType Directory = directory create karni hai
-Path ".\example"   = target
```

Existing target par error ya tool-specific behavior aa sakta hai. `-Force` behavior
change kar sakta hai, lekin blindly use nahi karna—existing item ko ignore/affect karne
ka risk pehle samjho.

## 4. Empty file create — `New-Item`

```powershell
New-Item -ItemType File -Path ".\example\task.txt"
```

Yeh empty file create karta hai. Parent directory absent ho to command fail ho sakti
hai; parent pehle create/verify karo.

Actual source/config content later relevant tools/editor se add hoga. Empty file create
hona correct application code prove nahi karta.

## 5. File content read — `Get-Content`

```powershell
Get-Content -LiteralPath ".\notes\BACKEND_ROADMAP.md"
```

Yeh file content read karta hai. Large/binary file ko blindly print karna terminal ko
noisy bana sakta hai. Secrets wali files ka content terminal output mein expose nahi
karna.

## 6. Copy — `Copy-Item`

```powershell
Copy-Item -LiteralPath ".\example\task.txt" `
  -Destination ".\example\task-copy.txt"
```

Copy original item preserve karke new item create karta hai:

```text
Before: task.txt
Copy
After:  task.txt + task-copy.txt
```

Destination collision aur overwrite behavior verify karo. Directory copy ke saath
`-Recurse` nested content include kar sakta hai; scope broad hone se pehle inspect karo.

Backtick above sirf PowerShell line-continuation character hai. Beginner ke liye command
ek line mein likhna less error-prone ho sakta hai.

## 7. Rename — `Rename-Item`

```powershell
Rename-Item -LiteralPath ".\example\task-copy.txt" `
  -NewName "task-renamed.txt"
```

Same parent directory mein item ka name change hota hai:

```text
task-copy.txt -> task-renamed.txt
```

References/imports old name use kar rahe hon to rename application break kar sakta hai.
Case-only rename cross-platform/Git behavior mein special care maang sakta hai.

## 8. Move — `Move-Item`

```powershell
Move-Item -LiteralPath ".\example\task-renamed.txt" `
  -Destination ".\archive"
```

Move item ki location change karta hai; source location par item normally nahi rehta:

```text
Before: example/task-renamed.txt
Move
After:  archive/task-renamed.txt
```

Destination directory existence, same-name collision aur exact resolved paths pehle
verify karo.

## Rename versus move

| Operation | Main intention | Source name/location after success |
|---|---|---|
| Copy | Duplicate banana | Original remains |
| Rename | Name change | Old name removed, new name present |
| Move | Location change | Old location normally empty |

Some tools move ke destination mein new filename dekar move + rename together kar
sakte hain. Explicit steps beginner debugging mein clearer hote hain.

## 9. Delete — `Remove-Item`

```powershell
Remove-Item -LiteralPath ".\example\task.txt"
```

Deletion destructive ho sakti hai aur normal command-line delete Recycle Bin mein
jana guaranteed nahi. Exact target, type and recoverability confirm karo.

Empty directory:

```powershell
Remove-Item -LiteralPath ".\example"
```

Non-empty directory ko recursively delete karne wala `-Recurse` scope bahut broad kar
sakta hai. Workspace/root/home jaise broad targets par unresolved variables, wildcard
ya guessed path ke saath recursive delete kabhi nahi.

## `-WhatIf` preview

Supported state-changing PowerShell cmdlets par `-WhatIf` intended operation preview
kar sakta hai without performing it:

```powershell
Remove-Item -LiteralPath ".\example\task.txt" -WhatIf
```

`-WhatIf` useful safety layer hai, lekin final target reasoning ka replacement nahi.
Har external command `-WhatIf` support kare, assume nahi karna.

## `-Confirm` prompt

Supported cmdlets mein `-Confirm` execution se pehle confirmation request kar sakta hai:

```powershell
Remove-Item -LiteralPath ".\example\task.txt" -Confirm
```

Automation/scripts interactive prompt par hang kar sakte hain; learning/manual risky
operation mein yeh useful ho sakta hai. Exact behavior command/provider par depend hai.

## `-Force` ko carefully samjho

`-Force` protections/visibility/existing-item behavior ko command-specific way mein
change kar sakta hai. Iska universal meaning “make it work safely” nahi hai.

Rule:

```text
Command help read karo -> target verify karo -> -Force ka exact effect samjho
```

## Aliases versus full names

PowerShell interactive aliases mil sakte hain, jaise `mkdir`, `copy`, `move`, `del`.
Cross-shell meanings differ kar sakte hain. Learning notes aur scripts mein explicit
cmdlets prefer karenge:

```text
New-Item
Copy-Item
Rename-Item
Move-Item
Remove-Item
```

Full names code review aur interviews mein intention clear rakhte hain.

## Safe operation lifecycle

```text
1. Get-Location se CWD verify
2. Target absolute/relative classify
3. Resolve exact parent/target where possible
4. Test-Path se before-state inspect
5. Operation ka destructive/overwrite risk identify
6. Supported ho to -WhatIf preview
7. One scoped operation execute
8. Test-Path/Get-ChildItem se after-state verify
9. Error ho to stop, output read, blind retry nahi
```

## Practical execution order used

```text
.topic22-practice absent verify
  -> source/ directory create
  -> archive/ directory create
  -> source/task.txt empty file create
  -> source/task-copy.txt copy create
  -> copy rename to source/task-renamed.txt
  -> renamed file move to archive/
  -> tree inspect
  -> individual files delete
  -> verified empty directories delete
  -> sandbox absence verify
```

Final practice tree before cleanup:

```text
.topic22-practice/
  archive/
    task-renamed.txt
  source/
    task.txt
```

## Common errors aur fixes

### Target already exists

Cause: create/copy/rename destination collision. Fix: `Test-Path` and listing se exact
before-state inspect karo; overwrite/delete assume mat karo.

### Parent directory missing

Cause: nested file destination ka parent absent. Fix: intended parent path verify/create
karo; typo ko new folder bana kar hide nahi karo.

### Path not found

Cause: wrong CWD, spelling, moved/renamed source or missing target. Fix: `Get-Location`,
`Resolve-Path`, `Test-Path` sequence use karo.

### Access denied / item in use

Cause: permissions, read-only state or another process file use kar raha ho sakta hai.
Force/administrator mode blindly use nahi; exact cause and ownership identify karo.

### Directory not empty

Cause: non-empty directory without recursive handling. Fix: children inspect karo.
`-Recurse` immediately add karna safe diagnosis nahi.

### Wrong item deleted

Cause: wrong base, wildcard, parent traversal or unchecked variable. Prevention: resolved
absolute target ko intended workspace boundary se compare karo before deletion.

## TaskForge connection

Future project mein operations use hongi:

```text
Create: src/, tests/, configuration files
Read:   package.json, logs, source code
Copy:   example configuration/template
Rename: module/file responsibility evolve hona
Move:   refactoring folder structure
Delete: obsolete generated/temp artifact after verification
```

Source rename/move ke baad imports, tests and documentation update/verify karne padenge.
Filesystem operation isolated action nahi; code relationships par impact ho sakta hai.

## Verified evidence

Actual isolated run results:

```text
CreatedOriginal=True
CopiedFile=True
RenamedFile=True
MovedFile=True
CleanupComplete=True
```

Safety evidence:

- target resolved to exact workspace child
  `C:\Users\ajaym\Desktop\Practicle\.topic22-practice`;
- target operation se pehle absent tha;
- only practice files/directories operate hue;
- cleanup individual files and empty directories par hua, broad recursive delete nahi;
- final practice target absent hai;
- existing documentation/application artifacts delete nahi hue.

## Student exercise

Ek disposable, clearly named practice directory mein—important data ke saath nahi—yeh
lifecycle khud explain aur perform karo:

1. before-state check;
2. directory and empty file create;
3. file copy;
4. copied file rename;
5. renamed file another practice directory mein move;
6. tree inspect;
7. `-WhatIf` delete preview;
8. exact practice items cleanup;
9. final absence verify.

Har step ke pehle predict karo aur baad mein evidence likho.

## Exercise answer template

```text
CWD:
Exact sandbox path:
Before exists:
Created file exists:
Copied file exists:
Renamed file exists:
Moved file exists:
WhatIf result:
Cleanup verified:
Error observed and reason:
```

## Interview question with Hinglish answer

**Question:** PowerShell mein file operations safely kaise perform karte ho?

**Answer:** Main pehle `Get-Location`, resolved target aur `Test-Path` se before-state
verify karta hoon. Create ke liye `New-Item`, copy ke liye `Copy-Item`, rename ke liye
`Rename-Item`, move ke liye `Move-Item` aur delete ke liye `Remove-Item` use hota hai.
State-changing operation mein collision, overwrite aur recursive scope check karta hoon,
supported ho to `-WhatIf` preview leta hoon, phir after-state verify karta hoon. Broad ya
unresolved target par recursive delete nahi karta.

## Easy-English minimum interview answer

**I verify the current location and exact target before changing files. I use explicit
PowerShell commands for create, copy, rename, move, and delete, preview risky operations
with `-WhatIf` when supported, and verify the result after every change.**

Short version:

**Check the target, perform one scoped file operation, and verify the result. Never run
a recursive delete on an unverified path.**

## Completion boundary

Topic 22 mein core file/directory operations, before/after state, lifecycle, errors and
destructive-operation safety complete hui. **Topic 23 — VS Code workspace** next hai aur
abhi start nahi hua.

