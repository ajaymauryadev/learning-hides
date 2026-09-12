# Topic 26 — Hidden files

## Learning goal

Hidden files/directories, Windows Hidden attribute, dot-prefixed convention, PowerShell
inspection, Git tracking, secrets and safe handling samajhna hai.

> Hidden visibility property hai; security, secrecy ya Git-ignore guarantee nahi.

## Hidden item kya hota hai?

Hidden item ko OS, shell ya editor default view mein attributes, naming convention or
view settings ke basis par omit kar sakta hai. Item filesystem par still exist karta
hai aur permission ho to exact path se access ho sakta hai.

```text
Hidden from default view != absent from filesystem
```

## Current repository evidence

Normal listing:

```powershell
Get-ChildItem -LiteralPath "."
```

Visible result:

```text
notes/
LEARNING_MEMORY.md
TASKFORGE_BACKEND_MASTER_PLAN.md
```

Hidden items include karke:

```powershell
Get-ChildItem -LiteralPath "." -Force
```

Additional `.git/` visible hua. Exact inspection:

```powershell
Get-Item -LiteralPath ".git" -Force |
  Select-Object Name, PSIsContainer, Attributes
```

Verified:

```text
Name          = .git
PSIsContainer = True
Attributes    = Hidden, Directory, NotContentIndexed
```

`NotContentIndexed` Windows indexing property hai, TaskForge feature nahi.

## Windows attribute versus dot convention

Windows item par actual `Hidden` attribute ho sakta hai. Unix/Linux mein leading dot
name commonly default listings se item hide karta hai:

```text
.env
.gitignore
.git/
```

Leading dot and Windows Hidden attribute separate mechanisms hain. Current `.git` ke
paas both dot-prefixed name and Hidden attribute hain. Har item ke liye ask karo:

```text
1. Name dot se start hota hai?
2. Windows Hidden attribute set hai?
3. Current tool/view item omit karta hai?
```

## PowerShell mein inspect karna

```powershell
Get-ChildItem -LiteralPath "." -Force
Get-Item -LiteralPath ".git" -Force
Test-Path -LiteralPath ".git"
```

Listing mein `-Force` hidden/system-like items include kar sakta hai. `-Force` ka effect
command-specific hai: `Get-ChildItem -Force` read-only visibility broad karta hai, while
state-changing command ke saath protection/behavior affect ho sakta hai. It does not
mean “perform safely.”

Broad `-Force -Recurse` listing huge/slow output, access warnings or sensitive filenames
show kar sakti hai. Exact scope use karo.

## Hidden, ignored, untracked aur secret

| Term | Meaning |
|---|---|
| Hidden | Default view mein omit ho sakta hai |
| Git-ignored | Git ignore rule matching untracked item ko normally omit karta hai |
| Untracked | File disk par hai but Git index/history mein add nahi |
| Tracked | Git file changes record/compare karta hai |
| Secret | Sensitive value requiring protection |

Relationships automatic nahi:

- hidden file tracked ho sakti hai;
- visible file ignored ho sakti hai;
- untracked item secret ho bhi sakta hai aur nahi bhi;
- `.env` dot-prefix se encrypted/ignored nahi hoti;
- `.gitignore` commonly tracked configuration hoti hai.

## `.git` directory

`.git/` repository metadata, objects, refs and configuration manage karti hai. Yeh source
folder nahi and “hidden so unimportant” nahi. Manually edit/delete karne se repository
state/history damage ho sakti hai. Git state ko Git commands through operate karna safer
abstraction hai. Current inspection ne contents modify nahi kiye.

## `.gitignore`

`.gitignore` patterns Git ko matching untracked files normally ignore karne ko kehti hain.

- ignore disk se delete nahi karta;
- encryption/access control nahi deta;
- already tracked file automatically untrack nahi hoti;
- broad pattern important files hide kar sakta hai;
- rule behavior `git status`/`git check-ignore` se verify hota hai.

Current root `.gitignore` existence ka claim nahi; need par later create/verify hogi.

## `.env` aur secrets

Future `.env` mein `DATABASE_URL`, `JWT_SECRET` or API keys ho sakti hain. Rules:

- real `.env` commit nahi;
- ignore coverage verify;
- `.env.example` mein placeholders only;
- output, logs, screenshots and errors mein values reveal nahi;
- production mein environment/secret management use;
- leak par credential rotate—hide/delete alone enough nahi.

Hidden status access control nahi. Filesystem access wala user/program still read kar
sakta hai.

## VS Code/File Explorer visibility

Item filesystem attribute, `files.exclude`, `search.exclude`, ignore-aware setting or UI
filter ke kaaran Explorer/search se absent ho sakta hai. Explorer mein missing item disk
par absent prove nahi karta. `Test-Path` and forced listing se verify karo.

Windows File Explorer ka “show hidden items” view change karta hai; actual attribute or
permissions remove nahi karta.

## Hidden name aur extension

`.env`/`.gitignore` ko `report.pdf` style suffix assumption se mat interpret karo.
Complete filename convention dekho. `.env.example` ka final extension `.example` report
ho sakta hai, while complete name environment template role communicate karta hai.

## Safe inspection flow

```text
1. Get-Location se CWD verify
2. Normal listing inspect
3. -Force listing compare
4. Exact item name/type/attributes inspect
5. Git tracked/ignored state separately verify
6. Content read se pehle secret risk assess
7. Unknown metadata item modify/delete nahi
```

## Common mistakes aur fixes

### Hidden means secure

Permissions, encryption, ignore policy and secret management separately required hain.

### Dotfile automatically ignored

Git status/rules verify karo. Filename alone tracking policy nahi.

### Explorer mein absent means deleted

Exact `Test-Path` and `Get-ChildItem -Force` evidence lo.

### `.git` manually edit/delete

Repository damage risk. Intended Git command and recoverability samjho.

### `.env.example` mein real secret

Placeholder use karo; exposed credential rotate karo.

### Hidden item archive/deployment mein leak

Package/deployment include/exclude rules inspect karo; `.env`/`.git` accidentally bundle
nahi hone chahiye.

## Observed Git warning

Status checks reported:

```text
warning: unable to access 'C:\Users\ajaym/.config/git/ignore': Permission denied
```

Git user-level/global ignore path access nahi kar pa raha. Repository status still
printed, but global ignore behavior incomplete ho sakta hai. Yeh `.git` Hidden attribute
failure nahi. User-level file current scope/workspace ke outside hai, so no mutation or
permission bypass attempted.

## TaskForge plan

```text
taskforge-backend/
  .gitignore       -> tracked ignore rules
  .env             -> local values/secrets, ignored
  .env.example     -> tracked safe template
  src/             -> application source
```

Exact files later ordered initialization mein. Topic 26 ne kuch create nahi kiya.

## Data/control flow

```text
filesystem item + name/attributes
  -> shell/editor visibility rules
  -> default or forced view
  -> permissions govern access
  -> Git independently governs tracking
```

## Verification evidence

```text
Normal listing: .git omitted
-Force listing: .git included
.git type: directory
.git attributes: Hidden, Directory, NotContentIndexed
```

No hidden item created, edited, deleted or attribute-changed.

## Student exercise

1. Normal and `-Force` root listings compare karo.
2. `.git` ka name/type/attributes inspect karo.
3. Hidden, ignored, untracked and secret separately define karo.
4. Explain why `.env` hidden-looking hone ke baad bhi secure nahi.
5. Explain why `.git` manually delete nahi karni.

## Exercise answer

```text
.git normal listing mein omitted, -Force mein visible
.git is a directory with Hidden attribute
Hidden = visibility
Ignored = Git rule
Untracked = Git index state
Secret = sensitive information
.env protection requires ignore/access/secret policy
.git deletion repository metadata/history damage kar sakti hai
```

## Interview question with Hinglish answer

**Question:** Hidden file kya hoti hai, aur kya woh secure ya Git-ignored hoti hai?

**Answer:** Hidden item default OS, shell ya editor view mein attribute/naming convention
ke cause omit ho sakta hai, but filesystem par exist karta hai. Hidden hona security,
encryption ya Git-ignore guarantee nahi. PowerShell mein `Get-ChildItem -Force` se hidden
items inspect karta hoon. `.env` secrets ko ignore rules, permissions and secret
management se protect karta hoon; `.git` metadata manually mutate nahi karta.

## Easy-English minimum interview answer

**A hidden file is omitted from some default views because of an attribute or naming
convention. Hidden does not mean secure or Git-ignored. In PowerShell, I use
`Get-ChildItem -Force` to inspect hidden items safely.**

Short version:

**Hidden is a visibility property, not a security or Git-tracking rule.**

## Completion boundary

Topic 26 mein hiding mechanisms, inspection, Git/secret boundaries, errors and safety
complete hue. **Topic 27 — Environment verification** next hai aur abhi start nahi hua.

