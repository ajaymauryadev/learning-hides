# Topic 21 — Absolute aur relative paths

## Learning goal

Aaj filesystem location ko two ways se address karna seekhenge: absolute path aur
relative path. Hum path resolution, Windows syntax, `.`/`..`, quoting, normalization,
wildcards aur safe command targeting samjhenge. File/folder create, copy, move, rename
aur delete operations Topic 22 mein aayengi.

## Path kya hota hai?

**Path filesystem mein file ya directory ki location ko represent karne wala address
hai.**

Example file:

```text
C:\Users\ajaym\Desktop\Practicle\notes\BACKEND_ROADMAP.md
```

Ismein:

```text
C:                   = Windows drive
\                    = path separator
Users, ajaym, ...    = directories/path segments
BACKEND_ROADMAP.md   = final file name
```

Path khud file content nahi; location ka reference hai.

## Absolute path

**Absolute path filesystem root/drive se complete location batata hai. Isko resolve
karne ke liye current working directory ki zaroorat nahi hoti.**

```text
C:\Users\ajaym\Desktop\Practicle\notes\BACKEND_ROADMAP.md
```

Properties:

- drive/root se start hota hai;
- complete location convey karta hai;
- CWD change hone par meaning normally same rehta hai;
- machine/user/folder structure change hone par portable nahi ho sakta.

Read-only check:

```powershell
Test-Path -LiteralPath "C:\Users\ajaym\Desktop\Practicle\notes\BACKEND_ROADMAP.md"
```

Result `True` ka matlab target currently exists; yeh content correctness prove nahi
karta.

## Relative path

**Relative path kisi base location—commonly current working directory—ke relation mein
location batata hai.**

Current CWD:

```text
C:\Users\ajaym\Desktop\Practicle
```

Relative path:

```text
notes\BACKEND_ROADMAP.md
```

Conceptual resolution:

```text
C:\Users\ajaym\Desktop\Practicle
  + notes\BACKEND_ROADMAP.md
  --------------------------------
C:\Users\ajaym\Desktop\Practicle\notes\BACKEND_ROADMAP.md
```

Relative path shorter aur project-portable hota hai, lekin uska meaning base/CWD par
depend karta hai.

## Absolute versus relative comparison

| Question | Absolute path | Relative path |
|---|---|---|
| Kahan se start? | Drive/root | Base directory, commonly CWD |
| CWD par dependent? | Normally no | Yes |
| Length | Usually longer | Usually shorter |
| Project portability | Often lower | Usually better |
| Exact target clarity | High | Base pata hona required |
| Example | `C:\Work\app\src` | `src` or `.\src` |

Absolute automatically better nahi, relative automatically unsafe nahi. Choice context
par depend karti hai.

## `.` ka meaning

Single dot current directory ko represent karta hai:

```text
.
.\notes
.\notes\BACKEND_ROADMAP.md
```

Current environment mein:

```text
.\notes\BACKEND_ROADMAP.md
```

aur:

```text
notes\BACKEND_ROADMAP.md
```

same target resolve karte hain. `.` explicit bana deta hai ki location current
directory se start ho rahi hai.

## `..` ka meaning

Double dot parent directory represent karta hai:

```text
Current: C:\Users\ajaym\Desktop\Practicle
Parent:  C:\Users\ajaym\Desktop
```

Example:

```text
..\Practicle\notes\BACKEND_ROADMAP.md
```

Current CWD se pehle parent par jaata hai, phir `Practicle\notes...` follow karta hai.

`..` powerful hai lekin repeated parent traversal wrong/broad target tak le ja sakta
hai. State-changing command se pehle resolved absolute target verify karo.

## Path resolution flow

```text
Command receives path
  -> path absolute hai?
      -> yes: root/drive se evaluate
      -> no: base/CWD ke against evaluate
  -> . and .. segments normalize
  -> target locate/check
  -> success result ya path-not-found error
```

## Path ko resolved absolute form mein inspect karna

Existing target ke liye:

```powershell
Resolve-Path -LiteralPath ".\notes\BACKEND_ROADMAP.md"
```

Expected resolved path:

```text
C:\Users\ajaym\Desktop\Practicle\notes\BACKEND_ROADMAP.md
```

`Resolve-Path` generally existing target resolve karta hai. Non-existing future path
ke liye failure aa sakta hai; failure ka matlab syntax always wrong nahi, target absent
bhi ho sakta hai.

## Paths safely join karna

Text ke saath manually slash concatenate karne ke bajay PowerShell path helper use kar
sakta hai:

```powershell
Join-Path -Path ".\notes" -ChildPath "BACKEND_ROADMAP.md"
```

Conceptual result:

```text
.\notes\BACKEND_ROADMAP.md
```

`Join-Path` segments ko intended separator ke saath combine karta hai. Join karna target
exist karta hai, yeh prove nahi karta; existence ke liye `Test-Path` use hota hai.

## Separator: backslash aur forward slash

Windows native path notation commonly backslash use karti hai:

```text
notes\BACKEND_ROADMAP.md
```

PowerShell/.NET ke many commands Windows par forward slash bhi accept kar sakte hain:

```text
notes/BACKEND_ROADMAP.md
```

Lekin har program/tool identical parsing kare, assume nahi karna. Windows/PowerShell
learning examples mein `\` use karenge; project docs and cross-platform tooling mein
forward slash commonly dikhega.

## Spaces wali path ko quote karo

```powershell
Test-Path -LiteralPath "C:\My Projects\TaskForge"
```

Quotes ensure karte hain ki space wali complete path one parameter value rahe. Is
example ka target hypothetical hai; lesson ne folder create nahi kiya.

## `-Path` versus `-LiteralPath`

PowerShell commands mein:

- `-Path` wildcard characters ko patterns ki tarah interpret kar sakta hai;
- `-LiteralPath` supplied text ko literal target ki tarah treat karta hai.

Example wildcard pattern:

```powershell
Get-ChildItem -Path ".\notes\*.md"
```

Example exact literal target:

```powershell
Get-Content -LiteralPath ".\notes\BACKEND_ROADMAP.md"
```

Exact known path aur safety-sensitive operation mein `-LiteralPath` ambiguity reduce
kar sakta hai.

## Wildcards path characters nahi, matching syntax hain

Common patterns:

```text
* = zero or more characters
? = usually one character
```

```powershell
Get-ChildItem -Path ".\notes\*.md"
```

Yeh single literal filename `*.md` nahi; matching Markdown files ka pattern hai.
Destructive operations ke saath wildcard ka scope carefully inspect kiye bina use nahi
karna.

## Path case aur portability

Windows filesystem commonly case-insensitive behavior deta hai, isliye `notes` aur
`NOTES` same target resolve kar sakte hain. Lekin Linux production systems commonly
case-sensitive hote hain. Backend project mein actual filename casing consistently
use karna portable habit hai.

```text
Local Windows: src/server.js and SRC/server.js may appear equivalent
Linux server:  they may be different paths
```

Exact behavior filesystem configuration par depend kar sakta hai; code mein casing
guess nahi karenge.

## Filesystem path versus URL

Filesystem path:

```text
C:\Projects\TaskForge\src\server.js
```

URL/API path:

```text
/api/tasks/123
```

Dono ko path kaha ja sakta hai, lekin first local filesystem address hai aur second
HTTP resource route. Inko mix nahi karna.

## Common errors aur debugging

### Wrong CWD

Relative path correct dikhti hai, lekin wrong base se resolve hoti hai.

```text
Check: Get-Location
Then: Resolve-Path/Test-Path with intended target
```

### Missing quotes

Space wali path multiple tokens ban sakti hai. Fix: complete value quote karo.

### File versus directory confusion

Path exist kar sakti hai, lekin expected type different ho sakta hai. Inspection se
check karo ki target file hai ya directory.

### Typo or wrong extension

`BACKEND_ROADMAP.md` aur `BACKEND-ROADMAP.md` different names hain. Exact listing aur
spelling verify karo.

### Non-existing target with `Resolve-Path`

`Resolve-Path` existing target na milne par error de sakta hai. Pehle `Test-Path` se
existence inspect karo; future path create karna Topic 22 ka operation hai.

### Too-broad parent/wildcard target

`..` ya `*` state-changing operation ka scope expected se broad bana sakte hain. Exact
resolved absolute target verify kiye bina delete/move nahi karna.

## Safe path verification sequence

```text
1. Get-Location se base verify karo
2. Path absolute ya relative classify karo
3. Quotes/wildcards inspect karo
4. Resolve-Path se existing target ka full form dekho
5. Test-Path se existence check karo
6. File/directory type and exact scope confirm karo
7. Tabhi state-changing command consider karo
```

## TaskForge connection

Future project root Topic 33 mein create hoga:

```text
taskforge-backend/
  package.json
  src/
  tests/
```

Project ke andar relative references—jaise `src/server.js`—repository ko doosre
developer/machine par portable rakhte hain. Deployment/configuration mein kuch absolute
paths environment-specific ho sakti hain. Code ko Ajay ke personal absolute desktop
path par hard-code nahi karenge.

## Verified evidence

Current CWD:

```text
C:\Users\ajaym\Desktop\Practicle
```

In references ne same existing file resolve ki:

```text
Absolute: C:\Users\ajaym\Desktop\Practicle\notes\BACKEND_ROADMAP.md
Relative: .\notes\BACKEND_ROADMAP.md
Parent-based: ..\Practicle\notes\BACKEND_ROADMAP.md
```

Verification used `Test-Path`, `Resolve-Path` and `Join-Path`; no file/folder state
changed.

## Student exercise

Current CWD se answers likho:

1. `notes\LEARNING_STATE.md` absolute hai ya relative?
2. Iska absolute version kya hoga?
3. `.\notes` mein `.` kya represent karta hai?
4. `..` kya represent karta hai?
5. `Join-Path` aur `Test-Path` ka difference kya hai?
6. Exact filename ke liye `-LiteralPath` kyun useful ho sakta hai?

## Exercise answer

```text
1. Relative path
2. C:\Users\ajaym\Desktop\Practicle\notes\LEARNING_STATE.md
3. Current directory
4. Parent directory
5. Join-Path segments combine karta; Test-Path existence check karta
6. -LiteralPath wildcard interpretation avoid karke exact text target karta
```

## Interview question with Hinglish answer

**Question:** Absolute aur relative path mein kya difference hai?

**Answer:** Absolute path root ya drive se complete filesystem location deta hai aur
normally CWD par depend nahi karta. Relative path base directory, commonly current
working directory, ke against resolve hota hai. Relative paths project ke andar zyada
portable hote hain, jabki absolute paths exact machine location clearly show karte hain
lekin environment-specific ho sakte hain. Main state-changing command se pehle CWD aur
resolved absolute target verify karta hoon.

## Easy-English minimum interview answer

**An absolute path gives the complete location from the filesystem root. A relative
path is resolved from a base location, usually the current working directory. Relative
paths are often more portable inside a project.**

Short version:

**An absolute path is a complete location. A relative path depends on the current or
specified base directory.**

## Completion boundary

Topic 21 mein path types, resolution, `.`/`..`, joining, quoting, wildcard safety,
errors and portability complete hue. **Topic 22 — File aur folder operations** next hai
aur abhi start nahi hua.

