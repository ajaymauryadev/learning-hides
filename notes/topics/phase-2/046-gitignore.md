# Topic 46 — `.gitignore`

## Learning goal

Ignore rule ka purpose, pattern scope, negation, tracked-file limitation aur secret-safety boundary
samajhna hai. TaskForge project folder mein minimum `.gitignore` add aur verify karna hai.

## Simple definition

**`.gitignore` Git ko batati hai ki kaunse untracked paths ko normal tracking candidates ke roop
mein show/stage nahi karna hai.**

```text
new untracked path
  -> ignore rules match?
     -> yes: normally hidden from status
     -> no: remains visible untracked candidate
```

`.gitignore` file delete, encrypt ya access-control nahi karti.

## TaskForge practical output

Created file:

```text
taskforge-backend/
`-- .gitignore
```

Rules:

```gitignore
node_modules/
.env
.env.*
!.env.example
coverage/
dist/
*.log
.DS_Store
Thumbs.db
```

File mein har rule group ke upar simple Hinglish comment reason explain karta hai.

## In paths ko ignore kyun kiya?

| Pattern | Reason |
|---|---|
| `node_modules/` | npm dependencies large/generated hoti hain; `package.json` aur lockfile se reinstall hongi |
| `.env`, `.env.*` | local secrets/config history mein nahi jaani chahiye |
| `!.env.example` | safe variable-name template team ke liye track ho sakti hai |
| `coverage/` | test tool generated report |
| `dist/` | future build output, source se recreate ho sakta hai |
| `*.log` | runtime/debug generated noise aur possible sensitive detail |
| `.DS_Store`, `Thumbs.db` | operating-system generated metadata |

`package-lock.json` ignore nahi kiya. Application repository mein reproducible dependency versions
ke liye lockfile normally commit hoti hai.

## Pattern basics

### Exact name

```gitignore
.env
```

Current `.gitignore` scope ke andar matching name ignore ho sakta hai.

### Directory pattern

```gitignore
node_modules/
```

Trailing slash directory intention clear karta hai.

### Wildcard

```gitignore
*.log
```

Matching log filenames ignore hote hain. Broad wildcard use karte waqt valuable source accidentally
hide na ho, yeh check karna chahiye.

### Negation

```gitignore
.env.*
!.env.example
```

`!` matching path ko previous ignore rule se include/unignore karne ki कोशिश करता है। Parent
directory ignored ho to sirf child negation sufficient na ho sakti; rule order aur parent matching
matter karte hain.

### Comment aur blank line

```gitignore
# human explanation
```

Leading `#` comment hota hai. Blank lines rule groups readable banati hain.

## Scope and location

`.gitignore` ke rules us directory aur descendants par apply hote hain. Current file
`taskforge-backend/.gitignore` mein hai, isliye TaskForge subtree ko describe karti hai:

```text
Practicle/
`-- taskforge-backend/
    |-- .gitignore
    |-- .env            -> ignored
    `-- src/app.js      -> not ignored by these rules
```

Parent `Practicle` repository currently TaskForge folder ko cover karti hai. Child `.gitignore`
ordinary trackable file hai; child `.git` repository create nahi hui.

## Rule sources

Git ignore decisions multiple sources se aa sakte hain:

```text
repository .gitignore files  -> team-shareable rules
.git/info/exclude            -> local repository-only rules
global excludes file         -> developer machine-wide rules
command-line patterns        -> particular command context
```

Team/project generated paths ke liye committed `.gitignore` useful hai. Personal editor-only
preference global exclude mein better ho sakti hai.

## Verification command

```powershell
git check-ignore -v --no-index taskforge-backend/.env
```

- `check-ignore` batata hai path ignore kyun hua.
- `-v` matching file, line aur pattern show karta hai.
- `--no-index` rule ko index state se independent test karne mein useful hai.

Safe hypothetical paths use karke rule test ho sakta hai; real `.env` secret create karna required
nahi.

Expected decisions:

```text
taskforge-backend/.env                  -> ignored
taskforge-backend/.env.development      -> ignored
taskforge-backend/.env.example          -> not ignored due to negation
taskforge-backend/node_modules/pkg/a.js -> ignored
taskforge-backend/coverage/index.html    -> ignored
taskforge-backend/dist/server.js         -> ignored
taskforge-backend/server.log             -> ignored
taskforge-backend/src/app.js             -> not ignored
taskforge-backend/package-lock.json      -> not ignored
```

## Ignored versus untracked versus tracked

```text
untracked + matching ignore rule -> ignored
untracked + no matching rule     -> ordinary untracked
already tracked file             -> ignore rule normally does not untrack it
```

Yeh last point critical hai. Secret pehle commit ho chuka ho aur baad mein `.gitignore` add karo,
to secret history se disappear nahi hota.

## Secret-safety boundary

`.gitignore` prevention layer hai, complete secret-management solution nahi:

- actual secret commit ho gaya ho to credential rotate/revoke karna padta hai;
- history exposure separately assess/clean karni pad sakti hai;
- `.env.example` mein secret values nahi, only safe placeholders hone chahiye;
- staging se pehle status/diff inspect karna zaroori hai;
- CI/CD aur deployment secrets secure platform configuration se aayenge.

Full secret incident workflow Topic 56 mein detail se hoga.

## `.gitignore` khud ignore nahi hoti

`.gitignore` normally repository mein track/commit ki jaati hai so team ko same project rules
milte hain. Rule file source-control policy ka part hai.

## Data flow

```text
working-tree path
  -> Git reads applicable ignore rules
  -> patterns evaluated in order
  -> ignored or visible-untracked classification
  -> status/add candidate presentation
```

No file delete, package uninstall, database query ya network upload hota hai.

## Execution order

```text
generated/secret paths identify
  -> minimum specific rules write
  -> safe hypothetical paths check-ignore se test
  -> important source/template paths non-ignored verify
  -> git status inspect
  -> later git add se reviewed files select
```

## Common errors aur explanations

### `.gitignore` add ki, lekin file still tracked hai

Ignore rules primarily untracked paths ke liye hain. Already tracked path ko rule alone untrack
nahi karta. File ka content/history sensitivity inspect karke deliberate remediation karo.

### `.env.example` bhi ignored ho gayi

Broad `.env.*` rule match karta hai. Uske baad `!.env.example` negation required hai; order matters.

### Rule expected path par apply nahi hui

`.gitignore` location/scope, slash semantics, spelling, case behavior aur later override inspect
karo. `git check-ignore -v --no-index <path>` matching source batata hai.

### Important file accidentally hidden

Pattern too broad ho sakta hai. Rule narrow karo and a representative important path explicitly
verify karo. `*.json` jaisi broad rule application project config/lockfiles hide kar sakti hai.

### `.gitignore` se committed secret safe samajhna

False. Past commit/remote copies remain kar sakti hain. Credential rotation first priority hoti hai.

### `node_modules` commit karna

Repository unnecessarily huge/noisy ban sakti hai, platform-specific generated content aa sakta
hai. Manifest and lockfile track karo; dependencies reinstall karo.

## Student exercise

1. `.gitignore` ka main purpose kya hai?
2. Kya ignored file disk se delete ho jati hai?
3. Kya later ignore rule already committed secret ko history se hata deti hai?
4. `.env.example` ko kyun allow kiya?
5. Kaunsa command matching rule source dikhata hai?
6. Kya `package-lock.json` ignore karna chahiye?

## Exercise answers

1. Selected untracked generated/private paths ko Git tracking candidates se exclude karna.
2. Nahi; file disk par remain karti hai.
3. Nahi; rotate/revoke and incident remediation needed hai.
4. Safe configuration template team ko required variable names bata sakti hai.
5. `git check-ignore -v --no-index <path>`.
6. Normal application project mein nahi; reproducible installs ke liye lockfile track hoti hai.

## Interview question with Hinglish answer

**Question:** `.gitignore` kya karta hai aur uski limitation kya hai?

**Answer:** `.gitignore` patterns define karti hai jisse Git matching untracked generated ya private
files ko normally status/staging candidates mein nahi dikhata. Yeh files delete ya encrypt nahi
karti, aur already tracked file ko automatically untrack nahi karti. Secret commit ho chuka ho to
ignore rule ke saath credential rotation aur history incident handling bhi zaroori hai.

## Easy-English minimum interview answer

> `.gitignore` tells Git which untracked files or folders should be ignored. It is commonly used
> for dependencies, generated files, logs, and local environment files. It does not remove an
> already tracked file or erase a committed secret from history.

## Completion boundary

Topic 46 mein TaskForge `.gitignore` create aur rules verify hue. Koi dependency, `.env` secret,
staging, commit ya remote operation nahi hui. Topic 47 — `git add` abhi start nahi hua.
