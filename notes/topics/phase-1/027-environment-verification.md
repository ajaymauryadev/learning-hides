# Topic 27 — Environment verification

## Learning goal

Development start karne se pehle assumptions ko commands/evidence se verify karna hai.
Hum environment layers, command resolution, PATH, expected-versus-actual comparison,
exit/error evidence aur repeatable checklist seekhenge.

Exact versions ordered next topics mein:

```text
Topic 28 -> Node version
Topic 29 -> npm version
Topic 30 -> Git version
Topic 31 -> VS Code version
```

Topic 27 mein version commands intentionally run nahi hui.

## Environment kya hai?

Development environment woh combined context hai jisme developer project build/run/test
karta hai:

```text
Operating system
  -> terminal and shell
  -> current directory and filesystem
  -> environment variables and PATH
  -> installed/resolvable tools
  -> project files/configuration
  -> permissions
  -> external services/network/credentials when required
```

Ek layer correct hone se complete environment correct prove nahi hota.

## Verification kya hai?

**Verification ka meaning expected condition ko observable evidence se check karke actual
result ke saath compare karna hai.**

```text
Expectation
  -> smallest relevant check
  -> actual output/status
  -> compare
  -> pass, fail or unknown
  -> next action
```

Example:

```text
Expectation: node command available hai
Check: Get-Command -Name node
Actual: node.exe ka resolved path
Conclusion: command resolvable; version/runtime behavior abhi unverified
```

## Installed, resolvable aur working ka difference

```text
Installed  = files/system package machine par present ho sakte hain
Resolvable = current shell command name ko locate kar sakti hai
Runnable   = process successfully start ho sakta hai
Compatible = version/project requirements match karti hain
Configured = required settings/credentials available hain
Healthy    = real/smoke operation expected result deti hai
```

`Get-Command node` pass hona sirf resolution evidence hai. It does not prove correct
version, application compatibility, database connectivity or TaskForge success.

## Evidence ladder

```text
1. Presence       -> file/command exists?
2. Resolution     -> shell kaunsa executable/script choose karegi?
3. Identity       -> tool/version kya hai?
4. Execution      -> smallest safe command run hoti hai?
5. Compatibility  -> project requirement match?
6. Integration    -> tool project/service ke saath kaam karta?
7. Health         -> meaningful end-to-end behavior correct?
```

Har topic mein required ladder level tak evidence lenge; “installed hai, so everything
works” conclusion nahi.

## Step 1 — CWD verify

```powershell
Get-Location
```

Current evidence:

```text
C:\Users\ajaym\Desktop\Practicle
```

Wrong CWD tool/project-file checks ko misleading bana sakti hai.

## Step 2 — Shell identify

```powershell
(Get-Process -Id $PID).ProcessName
$PSVersionTable.PSEdition
```

Current evidence:

```text
Shell process: pwsh
PowerShell edition: Core
```

PowerShell exact version Topic 18 evidence mein already recorded thi; Topic 27 ka focus
identity checklist hai, new version audit nahi.

## Step 3 — Command resolution

PowerShell:

```powershell
Get-Command -Name node
Get-Command -Name npm
Get-Command -Name git
Get-Command -Name code
```

`Get-Command` batata hai current shell name ko kaise resolve karegi. Useful fields:

```text
Name        -> resolved command name
CommandType -> Application, ExternalScript, Alias, Function, Cmdlet, etc.
Source/Path -> command origin/location
```

Current verified results:

```text
node -> FOUND | Application    | C:\Program Files\nodejs\node.exe
npm  -> FOUND | ExternalScript | C:\Program Files\nodejs\npm.ps1
git  -> FOUND | Application    | C:\Program Files\Git\cmd\git.exe
code -> FOUND | Application    | ...\Microsoft VS Code\bin\code.cmd
```

User-specific absolute VS Code path portability ke liye application code mein hard-code
nahi karenge.

## PATH ka beginner mental model

PATH environment variable directories ki ordered list hoti hai jahan shell external
command names search kar sakti hai.

```text
Developer types: node
  -> PowerShell command resolution
  -> aliases/functions/cmdlets/applications inspect
  -> PATH directories mein executable locate
  -> chosen command run
```

Important:

- tool disk par installed but PATH mein absent ho sakta hai;
- duplicate versions multiple directories mein ho sakti hain;
- order decide kar sakta hai kaunsi version choose hogi;
- new installation ke baad old terminal PATH refresh na kare; new shell needed ho sakti;
- PATH output share karte waqt personal/sensitive directory information consider karo.

## Same command name, different command type

Current evidence:

```text
node = Application
npm  = ExternalScript (npm.ps1)
git  = Application
code = Application (.cmd path resolved by PowerShell)
```

PowerShell execution policy/script behavior npm ke `.ps1` wrapper ko affect kar sakta
hai even when Node executable present ho. Exact failure output ke basis par diagnose
karna; random reinstall first step nahi.

## Step 4 — Project paths verify

```powershell
Test-Path -LiteralPath ".\notes" -PathType Container
Test-Path -LiteralPath ".\notes\BACKEND_ROADMAP.md" -PathType Leaf
Test-Path -LiteralPath ".\taskforge-backend"
```

Current results:

```text
notes directory: True
roadmap file: True
taskforge-backend: False
```

Last result expected hai because project root Topic 33 mein create hoga. Expected absence
failure nahi.

## Expected versus unexpected absence

```text
Expected absent + actual absent -> pass
Expected present + actual absent -> fail/blocker candidate
Expected unknown                -> requirement first define
```

Context bina `False` ko automatically error mat bolo.

## Step 5 — Repository state

```powershell
git status --short
```

Yeh working tree changes evidence deta hai. Current output Topic 19–26 ke uncommitted
learning changes show karta hai; they belong to current learning work and must be
preserved.

Git user-level ignore permission warning bhi aa rahi hai; Topic 26/DEBUG_LOG mein
documented. Warning ko ignore karke “clean environment” claim nahi karenge, but it did
not prevent repository status output.

## Step 6 — Version verification

General pattern:

```text
tool-specific version command
  -> actual version capture
  -> project-required range identify
  -> compatibility compare
```

Sirf newest version automatically correct nahi. Project may require LTS/range. Actual
commands and results Topics 28–31 mein one-by-one.

## Step 7 — Project-specific verification

Future TaskForge initialization ke baad:

```text
package.json exists
dependencies installed consistently
required environment variables valid
source entry point exists
tests/lint commands defined
configured port available
database/service reachable when needed
```

Abhi application absent hai, so application test/server start/database check claim nahi.

## Step 8 — External dependencies

Network, MongoDB Atlas, email provider etc. only real phase requirement par verify honge.
Tool availability and credentials separate checks hain:

```text
network reachable != authenticated
credential present != authorized
service reachable != correct database/data
```

Secrets verification output mein actual secret print nahi karna. Presence/validation
safe redacted form mein check karna.

## Environment verification matrix

| Layer | Question | Evidence example | Current status |
|---|---|---|---|
| Location | Correct folder? | `Get-Location` | verified |
| Shell | Expected shell? | process/PSEdition | `pwsh`, Core |
| Files | Required learning files? | `Test-Path` | verified |
| Node command | Resolvable? | `Get-Command node` | found |
| npm command | Resolvable? | `Get-Command npm` | found |
| Git command | Resolvable? | `Get-Command git` | found |
| VS Code CLI | Resolvable? | `Get-Command code` | found |
| Versions | Compatible? | version commands + requirement | Topics 28–31 |
| App root | Should exist now? | `Test-Path taskforge-backend` | expected absent |
| App health | Runs correctly? | future tests/smoke check | not applicable yet |

## Verification report format

```text
Check:
Expected:
Command/evidence:
Actual:
Status: PASS / FAIL / NOT APPLICABLE / UNKNOWN
Impact:
Next action:
```

This prevents vague statement: “setup theek lag raha hai.”

## Exit code, stdout and stderr

Topic 18 recap:

```text
stdout -> normal output
stderr -> diagnostic/error output
exit code 0 -> commonly success
non-zero -> commonly failure
```

But tool-specific semantics matter. Empty output success proof nahi; command exit state
and expected observable result inspect karo. Missing output ko pass claim nahi karna.

PowerShell `$LASTEXITCODE` recent native program exit code de sakta hai. Cmdlets often
PowerShell error mechanisms use karte hain. Exact error handling later deeper topic hai.

## Warning versus error

- warning: check may continue but condition attention maangti;
- error: operation/check fail ho sakta hai;
- informational output: state/evidence;
- absence of output: meaning command-specific, not automatic success.

Observed Git global-ignore warning is real warning, application runtime failure nahi.

## Common environment failures

### Command not recognized

Possible causes: tool absent, PATH missing/stale, typo, wrong shell. Check `Get-Command`,
installation source and new terminal session. Blind reinstall nahi.

### Wrong tool resolves

Multiple installations/PATH order. Capture resolved `Source`/`Path`, then intended tool
manager/project requirement compare.

### Version incompatible

Command exists but project-required range mismatch. Exact version + requirement evidence
needed.

### Works in one terminal, not another

Sessions have different PATH/CWD/profile/environment state. Both terminals independently
inspect karo.

### File not found

Wrong CWD/path, incomplete checkout or file not created yet. Expected state first define.

### Permission denied

Target ownership/access policy issue. Administrator/Force blindly use nahi; exact target
and required authority identify karo.

### Tool works, project fails

Tool installation sufficient nahi. Project dependencies, config, ports, services and
source errors separately diagnose.

## Safe troubleshooting order

```text
1. Exact error/output preserve and read
2. CWD and expected target verify
3. Command resolution/path capture
4. Version identity/requirement compare
5. Smallest safe command execute
6. Project configuration/dependency inspect
7. Permission/network/service layer only if evidence points there
8. Change one cause at a time
9. Same check rerun and record evidence
```

Reinstall everything first karna root-cause debugging nahi.

## Reproducibility

Good environment verification teammate/CI/new machine repeat kar sake:

- commands explicit;
- secrets absent/redacted;
- expected result documented;
- platform assumptions stated;
- versions eventually pinned/ranged;
- failures logged with evidence;
- no dependency on “mere machine par works.”

## TaskForge environment gate

Project root Topic 33 se pehle gate:

```text
PowerShell context known
CWD/path/file operations understood
hidden/config safety understood
node/npm/git/code commands resolvable
exact versions still must pass Topics 28–31
port/process basics must pass Topic 32
then project root create
```

## Student exercise

Without version commands, report banao:

1. CWD and shell identify karo.
2. `node`, `npm`, `git`, `code` resolution check karo.
3. each ka command type/path note karo.
4. notes/roadmap existence check karo.
5. `taskforge-backend` absence ko expected/failure classify karo.
6. “found” kya prove karta aur kya nahi, explain karo.

## Exercise answer

```text
CWD: C:\Users\ajaym\Desktop\Practicle
Shell: pwsh / PowerShell Core
node: found as Application
npm: found as ExternalScript (npm.ps1)
git: found as Application
code: found as Application/CLI command
notes and roadmap: present
taskforge-backend: expected absent until Topic 33
found proves resolution; version, compatibility and project health not yet proven
```

## Interview question with Hinglish answer

**Question:** Development environment ko systematically kaise verify karte ho?

**Answer:** Main pehle expected requirements define karta hoon, phir CWD, shell, required
files, command resolution path, versions, permissions and project-specific dependencies
layer-by-layer verify karta hoon. `Get-Command` se tool found hona sirf resolvability
prove karta hai, correct version ya working project nahi. Har check ka expected, actual,
status and evidence record karta hoon, aur failure par smallest relevant layer diagnose
karta hoon instead of blind reinstall.

## Easy-English minimum interview answer

**I verify a development environment layer by layer: working directory, shell, required
files, command resolution, tool versions, permissions, and project dependencies. Finding
a command only proves that the shell can resolve it; compatibility and project health
need separate checks.**

Short version:

**Environment verification compares expected requirements with command output and checks
each layer separately instead of assuming the setup works.**

## Completion boundary

Topic 27 mein verification method, environment layers, resolution, evidence ladder,
failure classification and TaskForge gate complete hua. **Topic 28 — Node version** next
hai aur abhi start nahi hua.

