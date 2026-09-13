# Topic 49 — `git diff --cached`

## Learning goal

Bahut simple way mein samajhna hai ki next commit ke liye kya select hua hai aur us selected content
ko commit se pehle kaise check karte hain.

## Sabse simple definition

**`git diff --cached` last commit aur staging area ko compare karta hai. Yeh dikhata hai ki agar hum
abhi commit karein, to proposed commit mein kya changes jayenge.**

```text
last commit (HEAD)
        ↕ git diff --cached
staging area (selected changes)
```

Command sirf review karta hai. Yeh खुद commit नहीं बनाता।

## Notebook wala easy example

Imagine karo:

```text
Old submitted answer       = HEAD / last commit
Next submission ke answers = staging area
Rough notebook             = working tree
```

- Rough notebook aur selected answers ka difference: `git diff`
- Old submitted answer aur selected answers ka difference: `git diff --cached`

Yaani `--cached` poochta hai:

> “Maine next submission ke liye exactly kya select kiya hai?”

## Basic command

```powershell
git diff --cached
```

`--staged` same meaning wala friendly alias hai:

```powershell
git diff --staged
```

Beginner ke liye dono equivalent samjho:

```text
git diff --cached = git diff --staged
```

## Topic start ki real state

Topic start par:

```text
staged files: 0
git diff --cached: empty
```

Reason simple hai: next commit ke liye kuch select hi nahi hua tha.

## Practical example

Topic 48 lesson already written aur reviewed thi. Sirf us exact file ko stage kiya:

```powershell
git add -- notes/topics/phase-2/048-git-diff.md
```

Ab:

```powershell
git diff --cached --name-status
```

Expected:

```text
A  notes/topics/phase-2/048-git-diff.md
```

`A` means new file staging area mein **Added** hai.

Full selected content dekhne ke liye:

```powershell
git diff --cached -- notes/topics/phase-2/048-git-diff.md
```

Isse commit se pehle verify hota hai ki correct file aur correct content selected hai.

## `git diff` versus `git diff --cached`

Easy comparison:

| Command | Kya compare karta hai? | Kya dikhata hai? |
|---|---|---|
| `git diff` | staging area ↔ working tree | अभी तक unstaged changes |
| `git diff --cached` | last commit ↔ staging area | next commit ke selected changes |

Example:

```text
file version in HEAD:    A
git add ke time version: B
later working version:   C
```

Then:

```text
git diff --cached -> A se B
git diff          -> B se C
```

Next commit currently version **B** lega, C नहीं। C को include करने के लिए review करके फिर add करना
होगा।

## Empty output ka meaning

```powershell
git diff --cached
```

Agar output empty hai, normally `HEAD` aur index same hain—koi staged content difference नहीं।

Lekin इसका मतलब repository clean होना जरूरी नहीं:

```text
unstaged changes हो सकती हैं
untracked files हो सकती हैं
ignored files हो सकती हैं
```

Isliye साथ में:

```powershell
git status --short
```

भी देखना चाहिए।

## Patch ko easy way mein kaise padhein

Example:

```diff
-Topic 49 planned
+Topic 49 complete
```

- `-` old committed side ki line hai.
- `+` staged/selected side ki line hai.

New file ke patch mein usually:

```text
new file mode 100644
--- /dev/null
+++ b/path/file.md
```

`/dev/null` yahan actual Windows folder नहीं। Git diff notation में इसका मतलब old side पर file नहीं
थी।

## Useful commands with examples

### Staged file names

```powershell
git diff --cached --name-only
```

Question answered: “Kaunse paths selected hain?”

### Staged file status

```powershell
git diff --cached --name-status
```

Question answered: “Selected path added, modified ya deleted hai?”

### Short size summary

```powershell
git diff --cached --stat
```

Question answered: “Commit ka size roughly kitna hai?”

### Exact file content

```powershell
git diff --cached -- path/to/file
```

Question answered: “Is selected file mein exact changes kya hain?”

### Whitespace problem check

```powershell
git diff --cached --check
```

Exit code `0` means selected patch mein configured whitespace error detect नहीं हुआ। यह automated
application test नहीं है।

## Safe commit se pehle checklist

```text
1. git status --short
2. git diff
3. git diff --cached --name-status
4. git diff --cached
5. git diff --cached --check
6. secrets aur unrelated files dobara check
7. then only git commit
```

Human shortcut:

```text
status = files ki list/state
plain diff = abhi unselected content
cached diff = selected commit content
```

## Secret check example

Cached diff mein aisa दिखे:

```text
DATABASE_PASSWORD=real-password
JWT_SECRET=real-secret
```

To commit मत करो। Staged selection correct karo, secret exposure assess karo aur `.gitignore`
policy fix karo. `git diff --cached` isi tarah last safety gate ban sakta hai.

## Data flow

```text
Git reads HEAD snapshot
  + reads staging-area snapshot
  -> compares both
  -> prints proposed commit patch
```

Working file edit नहीं होती, commit नहीं बनता और remote request नहीं जाती।

## Common errors aur easy fixes

### Cached diff empty hai

Kuch stage नहीं हुआ, ya staged state HEAD ke same hai. `git status --short` dekho.

### Latest edit cached diff mein नहीं है

File staging ke baad edit hui. Cached diff staged पुराना version दिखाएगा और plain diff later edit
दिखाएगा। Review karke dobara `git add` karo if latest edit include करनी है।

### Untracked file cached diff mein नहीं है

Untracked file staging area mein नहीं है। पहले content और secrets review करो, फिर intentionally
`git add -- exact/path` करो।

### Wrong file staged hai

Commit मत करो। Working content delete किए बिना unwanted path ko staging selection se safely हटाना
होगा, फिर status/cached diff दुबारा inspect करो।

### `--cached` ko browser cache samajhna

Yahan cached ka meaning Git index/staging area है, browser या Node cache नहीं।

### Diff देखकर commit assume karna

Cached diff केवल proposed commit दिखाता है। Durable commit अभी `git commit` के बाद बनेगा।

## Student exercise

1. `git diff --cached` किन दो states को compare करता है?
2. इसका simple question क्या है?
3. `git diff` और cached diff में difference क्या है?
4. File add करने के बाद फिर edit हुई तो latest edit cached diff में आएगी?
5. Staged file names देखने का command क्या है?
6. Cached diff में secret दिखे तो क्या करोगे?

## Exercise answers

1. Last commit/HEAD और staging area/index.
2. “Next commit के लिए exactly क्या selected है?”
3. Plain diff unstaged content; cached diff staged content दिखाता है।
4. नहीं, जब तक review करके file दोबारा stage न करें।
5. `git diff --cached --name-only`.
6. Commit रोकूँगा, selection/secret exposure fix करके फिर review करूँगा।

## Interview question with Hinglish answer

**Question:** `git diff --cached` क्या दिखाता है?

**Answer:** `git diff --cached` last commit यानी `HEAD` को staging area से compare करता है। इससे पता
चलता है कि अगला commit अभी exactly कौनसे selected changes contain करेगा। मैं commit से पहले इसे
review करता हूँ ताकि wrong file, incomplete change या secret commit न हो।

## Easy-English minimum interview answer

> `git diff --cached` compares the last commit with the staging area. It shows the changes currently
> selected for the next commit. I review it before committing to catch wrong files, incomplete
> changes, or secrets.

## Completion boundary

Topic 49 में reviewed Topic 48 lesson को stage करके cached diff verify हुआ। कोई commit, push,
restore, delete या application change नहीं हुआ। Topic 50 — `git commit` अभी start नहीं हुआ।
