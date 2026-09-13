# Topic 52 — `git remote`

## Learning goal

Easy examples se remote name aur URL mapping inspect karna hai. `git remote`, `-v`, `get-url`,
`show -n` aur configuration relationship samajhna hai. Existing `origin` ko mutate नहीं करना।

## Sabse simple definition

**`git remote` local repository mein saved remote names aur unke repository addresses ko inspect ya
manage karta hai.**

```text
remote name -> remote repository address
origin      -> https://github.com/.../repository.git
```

Command family configuration manage करती है। केवल `git remote` चलाने से code upload/download नहीं
होता।

## Phone contact wala easy example

Phone mein:

```text
Mummy  -> actual phone number
Office -> actual phone number
```

Git mein:

```text
origin   -> repository URL
upstream -> another repository URL (optional convention)
```

Contact name number नहीं है; उसी तरह `origin` repository URL नहीं, URL का local short name है।

## Basic command

```powershell
git remote
```

Current output:

```text
origin
```

Meaning: current local repository में one remote name configured है। यह command URL नहीं दिखाता।

## Name plus URLs

```powershell
git remote -v
```

Current output:

```text
origin  https://github.com/ajaymauryadev/learning-hides.git (fetch)
origin  https://github.com/ajaymauryadev/learning-hides.git (push)
```

`-v` means verbose. Current fetch और push URLs same हैं।

## Fetch URL aur push URL

Easy meaning:

```text
fetch URL -> changes/history कहाँ से लानी है
push URL  -> local commits कहाँ भेजनी हैं
```

They can be same or different. Current repository में same हैं। URL configured होना network access,
authentication या permission success prove नहीं करता।

Exact commands:

```powershell
git remote get-url origin
git remote get-url --push origin
```

## `origin` magic नहीं है

`origin` common default name है, especially clone के बाद, but mandatory keyword नहीं:

```text
origin    -> common primary remote name
upstream  -> common original-source name in fork workflow
company   -> team-chosen name possible
backup    -> another chosen name possible
```

Commands में configured exact name use करना पड़ता है।

## Current upstream relationship

Current local branch:

```text
main -> upstream origin/main
```

Read command:

```powershell
git rev-parse --abbrev-ref "main@{upstream}"
```

Output:

```text
origin/main
```

Three different things:

```text
origin       -> remote config name
main         -> local branch
origin/main  -> local cached remote-tracking reference
```

`origin/main` actual live server branch नहीं; last known local representation है।

## `git remote show -n origin`

```powershell
git remote show -n origin
```

`-n` network query skip करके locally configured/known details दिखाने के लिए use हुआ। Observed:

```text
Fetch URL: configured GitHub URL
Push URL:  configured GitHub URL
Local main pulls/merges with remote main
```

Without `-n`, `git remote show origin` remote query कर सकता है। Topic 52 verification intentionally
network-free है।

## Configuration actually कहाँ जुड़ी है?

Simplified local repository config:

```text
remote.origin.url   -> remote address
remote.origin.fetch -> remote branches ko local remote-tracking refs mein map karne ka rule
branch.main.remote  -> main ka remote name origin
branch.main.merge   -> upstream remote branch main
```

Observed fetch mapping:

```text
+refs/heads/*:refs/remotes/origin/*
```

Beginner meaning:

```text
remote branch refs/heads/<name>
        ↓ fetch mapping
local cached refs/remotes/origin/<name>
```

Leading `+` fetch update behavior control करता है; अभी इसे manually change नहीं करेंगे।

## Remote add ka syntax

Brand-new mapping syntax:

```powershell
git remote add origin https://host/owner/repository.git
```

Meaning:

```text
local name origin ko given URL se map karo
```

यह remote hosting site पर repository create नहीं करता। पहले actual remote repository और correct URL
exist होना चाहिए। Current `origin` already configured है, इसलिए command execute नहीं हुई।

## URL change ka syntax

```powershell
git remote set-url origin https://host/owner/new-repository.git
```

यह local mapping बदलता है, remote server repository move/delete/create नहीं करता। Wrong URL से future
fetch/push गलत destination पर जा सकता है, इसलिए exact target ownership verify करना जरूरी है।

## Rename ka syntax

```powershell
git remote rename origin upstream
```

Local remote name और related references/config update हो सकते हैं। Remote hosted repository rename
नहीं होती। Current setup में execute नहीं किया।

## Remove ka syntax

```powershell
git remote remove origin
```

Local remote mapping और associated remote-tracking refs हट सकते हैं। GitHub repository delete नहीं
होती। फिर भी local tracking context loss/commands break हो सकते हैं, इसलिए casual cleanup command
नहीं। Current setup में execute नहीं किया।

## `git remote` versus network commands

```text
git remote       -> mappings inspect/manage
git fetch        -> remote history download/update known refs
git pull         -> fetch + integrate
git push         -> local commits remote पर publish
```

Topic 52 mapping address book है। Topic 53 actual push करेगा/समझाएगा।

## Current TaskForge boundary

Current remote belongs to parent repository:

```text
Practicle/.git
  -> origin: ajaymauryadev/learning-hides.git
  -> contains learning notes and taskforge-backend child
```

यह independently named `taskforge-backend` Git repository का remote नहीं है। Child folder का own
`.git` नहीं। Future repository organization intentional decision होगी; अभी existing working setup
को silently replace नहीं करेंगे।

## URL safety

Remote URL display/share करने से पहले secret check करें। Unsafe example:

```text
https://username:secret-token@host/repository.git
```

Token/password URL में embed या docs/screenshots में expose नहीं करना चाहिए। Credential manager,
SSH key या platform-approved authentication use करें। If credential exposed, rotate/revoke करना
ज़रूरी हो सकता है। Current displayed HTTPS URL में embedded credential नहीं था।

## Multiple remotes example

Fork workflow:

```text
origin   -> your fork (push here)
upstream -> original project (fetch updates)
```

Names convention हैं। हमेशा `git remote -v` से actual URLs verify करो; नाम देखकर ownership assume
मत करो।

## Data/control flow

Read-only inspection:

```text
git remote command
  -> reads local .git/config and known refs
  -> formats name/URL/tracking output
```

Configuration mutation form:

```text
remote add/set-url/rename/remove
  -> updates local Git config/references
  -> does not automatically transfer commits
```

## Safe workflow

```text
git status --short --branch
  -> git remote
  -> git remote -v
  -> git remote get-url origin
  -> git remote get-url --push origin
  -> inspect upstream
  -> verify ownership and credential-free URL
  -> only then consider any mutation/network command
```

## Common errors aur easy fixes

### `error: remote origin already exists`

`origin` mapping पहले से configured है। Duplicate add मत करो। Existing `git remote -v` inspect करके
decide करो कि use, rename या carefully set-url करना है।

### `No such remote 'origin'`

Repository में उस exact name का remote नहीं। `git remote` से available names देखो। शायद different
name configured है या no remote exists।

### URL सही दिखी लेकिन push failed

URL config reachability/permission proof नहीं। Network, authentication, authorization, branch policy
या remote state issue हो सकता है। Exact error पढ़ो।

### `origin/main` को live GitHub branch समझना

यह cached local remote-tracking ref है। बिना fetch actual remote newer हो सकता है।

### `remote remove` से GitHub repository delete समझना

It removes local mapping, hosted repository नहीं। लेकिन local tracking info affect होती है।

### Token URL में डाल दिया

Credential logs/config/screenshots में leak हो सकता है। Token rotate/revoke और secure auth setup करो।

### Wrong repository पर push risk

Similar remote name misleading हो सकता है। Fetch और push दोनों URLs, account/owner और repository
नाम verify करो।

## Student exercise

1. `origin` क्या है—URL या local name?
2. `git remote -v` क्या extra दिखाता है?
3. Fetch और push URL का difference क्या है?
4. क्या `git remote add` GitHub repository create करता है?
5. `origin/main` live server branch है?
6. Existing remote URL बदलने से पहले क्या verify करोगे?

## Exercise answers

1. Remote URL का local short name.
2. Remote names के साथ fetch और push URLs.
3. Fetch read/download source; push publish destination.
4. नहीं, only local mapping create करता है।
5. नहीं, locally cached remote-tracking reference है।
6. Exact owner/repository, fetch/push destinations, credentials और intended workflow.

## Interview question with Hinglish answer

**Question:** `git remote` क्या करता है?

**Answer:** `git remote` local repository के remote names और URLs inspect/manage करता है। `origin`
एक conventional local name है, magic keyword नहीं। `git remote -v` fetch और push URLs दिखाता है।
Remote configure करना code transfer नहीं करता; fetch, pull और push separate network operations हैं।

## Easy-English minimum interview answer

> `git remote` displays or manages named connections to other Git repositories. A name such as
> `origin` is a local shortcut for a repository URL. Configuring a remote does not transfer commits;
> fetch, pull, and push are separate operations.

## Completion boundary

Topic 52 में existing `origin` name, URLs, upstream और local config read-only verify हुए। Remote
add/set-url/rename/remove, fetch, push या pull execute नहीं हुआ। Topic 53 — `git push` अभी start नहीं
हुआ।
