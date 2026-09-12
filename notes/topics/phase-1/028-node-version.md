# Topic 28 — Node version

## Learning goal

Installed Node.js runtime ki identity/version ko multiple evidence sources se verify,
version string ko read aur compatibility conclusions ko carefully limit karna seekhna
hai. npm version Topic 29 mein separately verify hogi.

## Node.js recap

Node.js JavaScript runtime hai jo browser ke outside JavaScript execute kar sakta hai.
Future TaskForge server process Node runtime par chalega.

```text
JavaScript source
  -> Node.js runtime
  -> running backend process
```

Node.js VS Code/editor, PowerShell/shell aur npm/package manager se different tool hai.

## Primary version commands

```powershell
node --version
node -v
```

Both current environment mein returned:

```text
v24.14.1
```

`--version` readable long option hai; `-v` short option. Scripts/docs mein clarity ke
liye long option useful hai.

## Version string anatomy

```text
v24.14.1
 |  |  |
 |  |  +-- patch = 1
 |  +----- minor = 14
 +-------- major = 24
```

Common semantic-version form:

```text
MAJOR.MINOR.PATCH
```

- **major:** compatibility-affecting release line/change ho sakta hai;
- **minor:** compatible features/improvements commonly;
- **patch:** compatible bug/security fixes commonly.

Leading `v` version marker hai. Comparison/storage mein some tools `v24.14.1`, others
`24.14.1` report kar sakte hain. Version rules/tool policy ko exact docs/range ke against
compare karna chahiye; numbers dekhkar universal compatibility assume nahi.

## Runtime ke andar se version

```powershell
node -p "process.version"
node -p "process.versions.node"
```

`-p` supplied JavaScript expression evaluate karke result print karta hai.

Verified:

```text
process.version       = v24.14.1
process.versions.node = 24.14.1
```

First leading `v` include karta hai; second numeric string return karta hai.

## Why multiple checks?

```text
node --version      -> CLI-reported identity
process.version     -> running Node process ki identity
Get-Command node    -> shell ne kaunsa command/path resolve kiya
process.execPath    -> running process ka actual executable path
```

In checks ka agreement PATH/multiple-install ambiguity reduce karta hai.

## Resolved executable evidence

```powershell
Get-Command -Name node
node -p "process.execPath"
```

Verified:

```text
Command type: Application
Resolved path: C:\Program Files\nodejs\node.exe
Runtime execPath: C:\Program Files\nodejs\node.exe
```

Shell resolution aur runtime executable same hain.

## Platform and architecture evidence

```powershell
node -p "process.platform"
node -p "process.arch"
```

Verified:

```text
platform: win32
architecture: x64
```

Node Windows ko `win32` platform identifier se report karta hai, including 64-bit
Windows. `x64` runtime architecture show karta hai. OS/platform and CPU architecture
dependency/native-addon compatibility mein matter kar sakte hain.

## Exit evidence

Final Node inspection returned native exit code:

```text
0
```

Common convention mein `0` success. Version string + executable path + exit code together
stronger evidence hain than “command screen par kuch print hua.”

## What is proven?

Current evidence proves:

- `node` current PowerShell se resolves;
- it launches successfully;
- running executable path identified;
- runtime reports Node `v24.14.1`;
- runtime platform/architecture `win32`/`x64`;
- checks agree.

It does **not** yet prove:

- TaskForge version requirement kya hogi;
- every future dependency Node 24-compatible hai;
- installed release line ka current support/LTS status;
- npm version or health;
- MongoDB/network availability;
- TaskForge application works.

## LTS, Current aur support status

Node release lines ka support/LTS status time ke saath change hota hai. Production version
choose karte waqt official current Node release schedule and dependency support verify
karna hoga. Topic 28 mein internet/current support lookup requested nahi tha, isliye
`v24.14.1` ko evidence ke bina “currently LTS” ya “best” label nahi diya.

General production preference commonly maintained, supported release line hoti hai—not
blindly highest number. Exact policy TaskForge initialization/deployment needs par decide
hogi.

## Version compatibility

Future `package.json` Node requirement declare kar sakta hai, conceptual example:

```json
{
  "engines": {
    "node": ">=24 <25"
  }
}
```

Yeh current project file nahi, only example. Range means minimum inclusive 24 and major
25 se below. Actual range dependencies, deployment platform and maintained release policy
verify karke set hogi.

Compatibility flow:

```text
installed version
  + declared project range
  + dependency support
  + deployment runtime
  -> compatible or mismatch decision
```

## Exact version versus range

- Exact version reproducibility improve kar sakti hai but updates intentionally manage
  karne padte hain.
- Version range compatible updates allow karti hai but environments drift kar sakte hain.
- Version manager file/container/deployment config runtime alignment help kar sakte hain.

We will introduce only justified tooling, not version-manager complexity for appearance.

## Multiple Node installations

Machine par multiple versions ho sakti hain:

```text
PATH entry A -> older node.exe
PATH entry B -> newer node.exe
```

`node --version` only selected executable ka version deta hai. Diagnose:

```powershell
Get-Command -Name node -All
node -p "process.execPath"
```

All discovered paths inspect karo. PATH reorder/uninstall blindly nahi.

## Terminal session and PATH refresh

Node install/switch ke baad already-open terminal old PATH/environment hold kar sakta hai.
New terminal start karke resolution/version repeat karna useful hai. VS Code restart only
when its environment needs refresh; assumption ke bajay compare evidence.

## Node version and running process

Running server jis Node process se start hua, wahi version use karta rahega. Machine par
later default Node switch karne se already-running process magically migrate nahi hota.
Restart/new process required hoga.

```text
Shell resolves Node A
  -> server process starts with Node A
  -> PATH later changes to Node B
  -> existing server still Node A until stopped/restarted
```

## Common errors aur fixes

### `node` not recognized

Possible: absent install, missing/stale PATH, typo. `Get-Command node`, installation and
fresh terminal inspect. Random project edits solution nahi.

### Wrong version prints

Possible: multiple installations/PATH order/version manager state. `Get-Command -All`,
`process.execPath` and shell session compare.

### Version command succeeds, project fails

Node presence/version only one layer. Dependency, module configuration, source syntax,
environment and project CWD inspect.

### Different version in VS Code versus external terminal

Sessions/environment initialization differ. Each terminal ka `Get-Command node`, version
and execPath capture.

### Unsupported syntax/API

Code/dependency feature installed Node version mein unavailable ho sakti hai. Required
version docs and actual runtime stack trace compare; blindly syntax rewrite nahi.

### Native dependency architecture issue

Platform/arch-specific package build mismatch ho sakta hai. `process.platform`,
`process.arch`, package installation evidence and supported matrix inspect.

## Safe version verification checklist

```text
1. Get-Location
2. Get-Command node
3. node --version
4. node -p "process.version"
5. node -p "process.execPath"
6. node -p "process.platform"
7. node -p "process.arch"
8. exit status capture
9. project requirement/deployment version compare when available
10. evidence record; unsupported conclusions avoid
```

## TaskForge decision record

Current development runtime candidate:

```text
Node: v24.14.1
Executable: C:\Program Files\nodejs\node.exe
Platform: win32
Architecture: x64
Status: installed/resolvable/runnable
Compatibility with future TaskForge: not yet formally declared/verified
```

No Node install, upgrade, downgrade or version switch performed. `package.json` and
TaskForge root still absent by curriculum order.

## Student exercise

Commands run karke report fill karo:

```text
node --version:
node -v:
process.version:
process.versions.node:
resolved command path:
process.execPath:
platform:
architecture:
exit code:
```

Then answer:

1. `v24.14.1` mein major/minor/patch kya hai?
2. Version command pass hone se TaskForge health prove kyun nahi?
3. Resolved path and execPath compare kyun karte hain?
4. Current LTS status ko memory se assume kyun nahi karna?

## Exercise answer

```text
CLI version: v24.14.1
process.version: v24.14.1
process.versions.node: 24.14.1
path/execPath: C:\Program Files\nodejs\node.exe
platform: win32
architecture: x64
exit: 0
major=24, minor=14, patch=1
version success only runtime identity proves, application health nahi
path comparison selected executable ambiguity reduce karta hai
support/LTS status time-sensitive hai; official current schedule verify hota hai
```

## Interview question with Hinglish answer

**Question:** Node.js version ko reliably kaise verify karte ho?

**Answer:** Main `node --version` se CLI version, `Get-Command node` se resolved command,
aur `node -p "process.version"`/`process.execPath` se actual running runtime identity
verify karta hoon. Major-minor-patch ko project `engines`, dependency support and deployment
runtime ke against compare karta hoon. Command pass hona application compatibility ya
health prove nahi karta, aur current LTS/support status official source se time par verify
karna chahiye.

## Easy-English minimum interview answer

**I verify Node.js with `node --version`, check the resolved executable path, and confirm
the running process version. Then I compare the major, minor, and patch version with the
project and deployment requirements.**

Short version:

**`node --version` shows the selected Node.js version, but project compatibility must be
checked separately.**

## Completion boundary

Topic 28 mein Node version, executable identity, version anatomy, compatibility limits,
platform and troubleshooting complete hue. **Topic 29 — npm version** next hai aur abhi
start nahi hua.

