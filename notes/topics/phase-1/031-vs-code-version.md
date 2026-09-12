# Topic 31 — VS Code version

## Learning goal

VS Code CLI version, resolved launcher, product build commit and architecture verify karni
hai. CLI/editor/extensions/project-runtime ko separate version layers samajhna hai. Port
aur process ka introduction Topic 32 mein aayega.

## VS Code recap

VS Code code editor/development environment hai. It can provide Explorer, editor,
integrated terminal, Git UI, diagnostics, debugger and extensions.

```text
VS Code -> development/editor tool
Node.js -> TaskForge JavaScript runtime
npm     -> package manager
Git     -> version control
```

VS Code version Node/npm/Git versions decide nahi karti.

## Primary CLI command

```powershell
code --version
```

Verified output three lines:

```text
1.137.0
645f29cc3176500b4b5762ba887cf2a7f0ffdf2c
x64
```

Exit code:

```text
0
```

## Output ko line-by-line samjho

```text
Line 1 -> VS Code product version: 1.137.0
Line 2 -> VS Code build/source commit identifier
Line 3 -> application architecture: x64
```

Version command output format future releases/platforms mein differ ho sakta hai. Parser
banate waqt blindly exactly three lines universal assume nahi karna.

## Version anatomy

```text
1.137.0
|  |  |
|  |  +-- patch = 0
|  +----- minor = 137
+-------- major = 1
```

VS Code product release numbering commonly this three-part form use karti hai. General
semantic-version intuition helpful hai, but vendor's actual compatibility/release policy
official documentation se decide hoti hai.

## Build commit kya hai?

```text
645f29cc3176500b4b5762ba887cf2a7f0ffdf2c
```

Yeh VS Code application build ko source revision se identify karta hai. Yeh current
TaskForge Git commit nahi.

```text
VS Code build commit -> editor product ka build source
TaskForge Git commit -> humare repository snapshot/history
Git build commit     -> installed Git binary ka build source
```

Same hex-looking format context same nahi banata.

## Architecture `x64`

`x64` 64-bit x86 application build indicate karta hai. Extension native component or
debugger/runtime integration troubleshooting mein editor architecture matter kar sakti
hai.

Current related evidence:

```text
VS Code: x64
Node: x64
Git build: x86_64
```

`x64` and `x86_64` tool-specific labels hain for same broad architecture family. Still,
each tool identity independently verified hai.

## Command resolution

```powershell
Get-Command -Name code -All
```

Current results:

```text
Selected:
C:\Users\ajaym\AppData\Local\Programs\Microsoft VS Code\bin\code.cmd

Alternate launcher:
C:\Users\ajaym\AppData\Local\Programs\Microsoft VS Code\bin\code
```

PowerShell normal invocation first resolved `code.cmd` chooses. User-specific install
path indicates per-user location; application source/config mein hard-code nahi karenge.

## CLI launcher kya karta hai?

```text
Developer types: code --version
  -> PowerShell resolves code.cmd
  -> launcher installed VS Code CLI/application ko argument pass karta hai
  -> version/build/architecture output
  -> exit code returns
```

CLI launcher and graphical editor same installation family ko target kar sakte hain, but
multiple installations/channels possible hain. Exact selected path isliye capture hota
hai.

## CLI version versus visible GUI version

Possible mismatch reasons:

- Stable and Insiders both installed;
- old terminal PATH;
- different user installation;
- portable/system/user installs;
- remote VS Code server/context;
- launcher points to different installation.

GUI verification commonly Help/About product details show kar sakti hai. Current topic
CLI evidence uses `code --version`; visible GUI state inspect/claim nahi hui.

## VS Code Stable versus Insiders

Different channels separate commands/installations use kar sakte hain, for example stable
`code` and Insiders commonly another launcher. Channel assumptions name/path/output se
verify honi chahiye. Current evidence selected path says Microsoft VS Code directory;
installed alternate channels ka broad search nahi hua.

## Extension versions separate hain

VS Code extension apna independent version/compatibility requirement rakhti hai:

```text
VS Code product version
  + extension version
  + extension engine requirement
  + project/runtime version
  -> feature compatibility
```

Editor version pass hone se ESLint/formatter/debug extension installed, enabled, trusted
or working prove nahi.

Extensions application dependencies nahi by default. TaskForge should CLI scripts/tests
se reproducibly verify ho, sirf one developer editor extension se nahi.

## Workspace and version are separate

Topic 23 workspace context:

```text
VS Code installed/runnable
  != correct folder open
  != integrated terminal correct CWD
  != files saved
  != project healthy
```

Version success ke baad bhi `Get-Location`, Explorer root, Git diff and tests separately
verify karne hain.

## What current evidence proves

- `code` current PowerShell mein resolvable hai;
- selected launcher path identified hai;
- CLI successfully launches version request;
- product reports `1.137.0`;
- build commit identified;
- application architecture `x64`;
- native exit code `0`.

It does not prove:

- currently visible window uses same install;
- correct TaskForge folder open hai;
- extension set safe/compatible hai;
- integrated terminal CWD correct hai;
- editor is latest or supported;
- Node/npm/Git or application healthy hain;
- source files saved/tested hain.

## Update and compatibility policy

“Latest VS Code version” time-sensitive fact hai. Current official source check kiye bina
`1.137.0` ko latest label nahi. Upgrade decision mein:

```text
current installed version
  + security/support information
  + extension compatibility
  + team policy
  + settings sync/backup/recovery
  -> update decision
```

Current topic ne editor install/update/downgrade nahi kiya.

## Remote development context

SSH/container/WSL-like remote development mein local VS Code UI aur remote server/runtime
different versions/environments rakh sakte hain. Current TaskForge local Windows context
hai. Remote tools real deployment/development need par introduce honge.

## Command options preview

```powershell
code --help
```

Available CLI options inspect kar sakta hai. Help/version read-only identity operations
hain. `code .` current directory editor mein open/change window state kar sakta hai; this
topic ne GUI launch/navigation perform nahi ki.

## Common errors aur fixes

### `code` not recognized

VS Code installed ho sakta hai but CLI PATH entry absent/stale. `Get-Command code -All`,
install location and new terminal inspect. Editor reinstall first response nahi.

### Wrong VS Code version

Multiple channels/installations or PATH order. Selected launcher, GUI About details and
terminal session compare.

### CLI works, extension fails

Extension version, enablement, trust, logs/output, engine requirement and project config
inspect. Editor version alone insufficient.

### CLI works, project files missing

Wrong workspace/CWD. Explorer root and `Get-Location` verify.

### Editor shows change, tests see old code

Unsaved buffer. Save state and actual disk diff inspect.

### Architecture mismatch

Native extension/tool architecture incompatibility. VS Code arch, OS arch, runtime arch
and extension support matrix compare.

### Update introduced behavior change

Release notes/settings/extensions and reproducible steps inspect; blindly downgrade or
delete settings nahi.

## Safe verification checklist

```text
1. Get-Location
2. Get-Command code -All
3. code --version
4. product version/build commit/architecture capture
5. exit code inspect
6. GUI version only if relevant, compare explicitly
7. extensions/workspace/CWD separately verify
8. current support/update info only official current source se
9. no install/update/config mutation during identity check
```

## TaskForge editor record

```text
VS Code CLI: 1.137.0
Selected launcher: ...\Microsoft VS Code\bin\code.cmd
Build commit: 645f29cc3176500b4b5762ba887cf2a7f0ffdf2c
Architecture: x64
Exit: 0
Workspace metadata: no .vscode/.code-workspace currently required
TaskForge root: absent until Topic 33
```

No extension installed/uninstalled, editor setting changed or GUI window opened.

## Student exercise

Report fill karo:

```text
code --version line 1:
line 2:
line 3:
selected launcher:
all launchers:
exit code:
```

Then explain:

1. version/build commit/architecture difference;
2. VS Code build commit vs TaskForge commit;
3. CLI version success workspace health kyun prove nahi;
4. extension versions separate kyun hain;
5. latest claim official verification kyun needs.

## Exercise answer

```text
version: 1.137.0
build: 645f29cc3176500b4b5762ba887cf2a7f0ffdf2c
architecture: x64
selected: user-local Microsoft VS Code bin/code.cmd
alternate: extensionless code launcher
exit: 0
build commit editor product ka; TaskForge commit project history ka
workspace/CWD/extensions/tests separate checks hain
extensions independent versions/engine requirements rakhte hain
latest/support time-sensitive; official current source required
```

## Interview question with Hinglish answer

**Question:** VS Code version ko reliably kaise verify karte ho?

**Answer:** Main `code --version` se product version, build commit and architecture capture
karta hoon, `Get-Command code -All` se selected launcher/multiple installs inspect karta
hoon, and exit code check karta hoon. CLI version success correct workspace, saved files,
extensions, terminal CWD or project health prove nahi karta. GUI mismatch ho to About
details and launcher path compare karta hoon.

## Easy-English minimum interview answer

**I verify VS Code with `code --version`, which reports the product version, build commit,
and architecture. I also inspect the resolved CLI path because multiple installations can
exist. Editor extensions and project health require separate checks.**

Short version:

**`code --version` identifies the selected VS Code installation, but it does not verify
the workspace or the application.**

## Completion boundary

Topic 31 mein VS Code product/build/architecture identity, launcher resolution,
GUI/extensions/workspace boundaries and errors complete hue. **Topic 32 — Port aur process
ka basic introduction** next hai aur abhi start nahi hua.

