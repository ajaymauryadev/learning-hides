# Topic 29 — npm version

## Learning goal

npm CLI ki identity/version reliably verify karni, npm ko Node runtime se distinguish
karna, PowerShell wrapper resolution samajhna aur evidence limits identify karna hai.
Git version Topic 30 mein separately verify hogi.

## npm kya hai?

**npm JavaScript/Node ecosystem ka package manager and command-line tool hai.** Future
TaskForge mein npm primarily:

- project manifest initialize/manage karega;
- dependencies install karega;
- scripts run karega;
- dependency resolution/lockfile maintain karega;
- package metadata inspect karega.

```text
Node.js -> JavaScript runtime
npm     -> package manager/CLI
```

npm commands execute karne ke liye installed Node runtime use karta hai, lekin Node and
npm independently versioned tools hain.

## Primary version commands

```powershell
npm --version
npm -v
```

Verified results:

```text
npm --version = 11.11.0
npm -v        = 11.11.0
exit codes    = 0
```

Both forms agree. `--version` readable long option, `-v` short option hai.

## Version anatomy

```text
11.11.0
 |  | |
 |  | +-- patch = 0
 |  +---- minor = 11
 +------- major = 11
```

Common semantic version form:

```text
MAJOR.MINOR.PATCH
```

npm ka major version Node ke major version se match hona required nahi:

```text
Node = 24.14.1
npm  = 11.11.0
```

Yeh contradiction nahi. They are separate products with separate release histories.

## PowerShell command resolution

```powershell
Get-Command -Name npm -All
```

Current environment found:

```text
ExternalScript -> C:\Program Files\nodejs\npm.ps1
Application    -> C:\Program Files\nodejs\npm.cmd
Application    -> C:\Program Files\nodejs\npm
```

Current PowerShell normal `npm` invocation mein first resolved match `npm.ps1` use karta
hai. Other shells may select `npm.cmd` or another launcher.

## Wrapper kya hota hai?

Wrapper small script/launcher hota hai jo actual npm CLI ko correct Node runtime and
arguments ke saath start karta hai:

```text
Developer types npm
  -> PowerShell resolves npm.ps1
  -> wrapper Node/npm CLI ko arguments pass karta hai
  -> npm operation runs
  -> output + exit code return
```

Isliye `npm` command type `ExternalScript` dikhna npm fake/broken hone ka proof nahi.

## npm and Node relationship

Node installation commonly npm bundle/provide kar sakti hai, but versions independent
rehte hain. npm update karne se Node major automatically update ho, aur Node update se
desired npm policy automatically match ho—assume nahi.

Compatibility decision:

```text
installed Node version
  + installed npm version
  + npm-supported Node range
  + project/deployment policy
  -> usable toolchain
```

Current release compatibility/support claims time-sensitive hain. Project initialize
karte waqt official requirements and actual operations verify hongi.

## What current evidence proves

- PowerShell `npm` resolve kar sakta hai;
- selected `npm.ps1` wrapper location known hai;
- npm process successfully runs version command;
- current npm reports `11.11.0`;
- long/short checks agree;
- version checks exit `0`.

It does not prove:

- future `package.json` valid hai;
- registry/network reachable hai;
- dependency install successful hoga;
- every package compatible/safe hai;
- TaskForge scripts/tests exist or pass;
- npm is latest/best version.

## `npm prefix` evidence

```powershell
npm prefix
```

Current result:

```text
C:\Users\ajaym\Desktop\Practicle
```

Exit code `0`. But current folder mein `package.json` absent hai. Therefore output ko
TaskForge initialized project root proof nahi bolenge. Command context and manifest
existence separately verify:

```powershell
Test-Path -LiteralPath ".\package.json"
```

Current result: `False`.

## Local versus global packages

Beginner mental model:

```text
Local dependency  -> specific project ke package.json/node_modules context
Global package    -> machine/user CLI use ke broader installation context
```

Backend application dependencies normally local project dependencies honi chahiye so
manifest/lockfile team and CI ko versions reproduce karne dein. Global install ko hidden
project dependency nahi banayenge.

## Manifest, lockfile and installation directory

Future:

```text
package.json      -> declared metadata, scripts and dependency ranges
package-lock.json -> resolved dependency tree/version integrity record
node_modules/     -> installed package files
```

npm version command in files ko create nahi karta. Topic 29 mein no initialization or
package installation performed.

## Dependency versus dev dependency

Conceptual preview:

- production dependency: application runtime needs;
- development dependency: tests/lint/dev tooling needs.

Exact commands/classification npm phase mein real TaskForge need ke saath cover hongi.
Package sirf tutorial list badhane ke liye install nahi hoga.

## npm scripts

Future `package.json` scripts names map to commands:

```text
npm run dev
npm test
```

Security rule: untrusted repository/package scripts code execute kar sakte hain.
`package.json`, lockfile and package origin inspect kiye bina blindly install/run nahi.

## Install security boundary

`npm install` external packages/downloads and lifecycle scripts execute kar sakta hai,
files create/change kar sakta hai, network use kar sakta hai. Version verification
read-only tha; package install nahi.

Before future install:

```text
correct project CWD
package identity/version
need and maintenance/security
dependency type
manifest/lockfile diff
install/test output
```

## PowerShell execution-policy error

Because PowerShell may resolve `npm.ps1`, some machines error de sakti hain ki script
execution disabled hai. Diagnose:

```powershell
Get-Command npm -All
```

Exact policy/error inspect karo. Security policy globally weaken karna default fix nahi.
`npm.cmd` alternative behavior environment policy ke according test ho sakta hai, but
current environment version command already succeeded.

## Multiple npm installations

Possible sources:

```text
Node installer bundled npm
version manager controlled npm
manually/global updated npm
different PATH entries
```

Wrong version diagnose:

```powershell
Get-Command npm -All
npm --version
Get-Command node
node --version
```

Selected wrappers and Node path compare; blindly delete/reinstall nahi.

## npm cache/registry are separate layers

npm version success local CLI identity prove karta hai. Registry DNS/network, proxy,
certificate, authentication and cache integrity separate checks hain. Install failure
par exact error classify karo:

```text
command resolution
Node/npm compatibility
project manifest
network/DNS/proxy
registry/authentication
package/version existence
permissions/filesystem
lifecycle script
```

## Common errors aur fixes

### npm not recognized

`Get-Command npm -All`, Node installation and PATH/session inspect.

### Script execution disabled

Selected `.ps1` wrapper and exact PowerShell policy error inspect; security controls
blindly disable nahi.

### Wrong npm version

All resolved launchers/PATH/version-manager state compare. Node version same assume nahi.

### `package.json` not found

CWD and manifest existence verify. Version command working hona project initialized hone
ka proof nahi.

### Install permission error

Local/global destination distinguish; administrator mode/Force first reaction nahi.

### Dependency conflict

Requested package ranges, peer requirements, Node/npm version and lockfile evidence read.
`--force` problem hide/broaden kar sakta hai; root cause understand karo.

### Version command succeeds but install fails

Expected: they verify different layers. Install error ka network/manifest/package/script
layer independently diagnose.

## Safe npm verification checklist

```text
1. Get-Location
2. Get-Command npm -All
3. npm --version
4. npm -v
5. exit codes inspect
6. node --version separately know
7. package.json existence check
8. project requirement/support compare when declared
9. install health only when authorized/needed
10. evidence record
```

## Current TaskForge toolchain record

```text
Node: v24.14.1
npm: 11.11.0
Selected npm command: C:\Program Files\nodejs\npm.ps1
npm version exit: 0
package.json: absent by curriculum order
TaskForge project root: absent until Topic 33
```

No package installed, updated, removed or published.

## Student exercise

Report fill karo:

```text
npm --version:
npm -v:
selected command type/path:
all npm launchers:
exit codes:
current prefix output:
package.json exists:
```

Then explain:

1. Node and npm versions different kyun ho sakti hain?
2. `npm --version` kya prove nahi karta?
3. `.ps1` wrapper ka role kya hai?
4. npm install ko harmless read-only command kyun nahi maan sakte?

## Exercise answer

```text
npm versions: 11.11.0 / 11.11.0
selected: ExternalScript, C:\Program Files\nodejs\npm.ps1
other launchers: npm.cmd and npm
version exits: 0
prefix: C:\Users\ajaym\Desktop\Practicle
package.json: False
Node/npm separate products and versions hain
version only CLI identity/runnability proves
wrapper actual CLI ko Node/arguments ke saath launches
install network/files/scripts and dependency state change kar sakta hai
```

## Interview question with Hinglish answer

**Question:** npm version ko kaise verify karte ho aur Node version se iska kya relation
hai?

**Answer:** Main `npm --version` and `npm -v` compare karta hoon, `Get-Command npm -All`
se selected wrapper and duplicate launchers inspect karta hoon, and exit code check karta
hoon. npm Node ecosystem ka package manager hai but Node se separately versioned hai.
Version command package installation, registry health, manifest validity ya project health
prove nahi karta; woh separate checks hain.

## Easy-English minimum interview answer

**I verify npm with `npm --version`, inspect the resolved command path, and check the exit
code. npm uses Node.js, but it has its own independent version. A successful version
check does not prove that package installation or the project will work.**

Short version:

**`npm --version` shows the selected npm CLI version. Node.js and npm have separate
version numbers and responsibilities.**

## Completion boundary

Topic 29 mein npm identity, wrapper resolution, version anatomy, Node relationship,
security and errors complete hue. **Topic 30 — Git version** next hai aur abhi start
nahi hua.

