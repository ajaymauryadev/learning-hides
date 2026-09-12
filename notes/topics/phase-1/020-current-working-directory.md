# Topic 20 — Current working directory

## Learning goal

Aaj samajhna hai ki terminal command kis folder ke context mein run hoti hai, current
working directory ko reliably kaise inspect karte hain, aur wrong location se command
run karne par backend project mein problems kyun aati hain. Absolute aur relative paths
Topic 21 mein detail se cover honge.

## Simple definition

**Current working directory—CWD—woh folder hai jise running shell/process apni current
filesystem location ya base location maanta hai.**

PowerShell mein current location inspect karne ka clear command:

```powershell
Get-Location
```

Current verified result:

```text
C:\Users\ajaym\Desktop\Practicle
```

Iska matlab abhi PowerShell commands ka working context `Practicle` folder hai.

## Naam ko word-by-word samjho

```text
Current   = abhi selected
Working   = command/process ke kaam ka context
Directory = folder
```

Directory aur folder beginner level par same filesystem concept ke do common words
hain. Command-line/documentation mein `directory` term zyada dikhegi.

## CWD file nahi hoti

CWD ek directory location hoti hai. Yeh currently open editor file ya terminal mein
last displayed file nahi hoti.

```text
Current file being edited: notes/LEARNING_STATE.md
Current working directory: C:\Users\ajaym\Desktop\Practicle
```

Dono related ho sakte hain, lekin same concept nahi hain.

## CWD kyun important hai?

Bahut se commands ko relative target diya jata hai:

```powershell
Get-ChildItem -Path notes
```

PowerShell `notes` ko current working directory ke context mein locate karta hai:

```text
CWD
  C:\Users\ajaym\Desktop\Practicle
      + relative target notes
  --------------------------------
  C:\Users\ajaym\Desktop\Practicle\notes
```

Yeh conceptual combination hai. Exact path-resolution rules Topic 21 mein aayenge.

## CWD kaise inspect karein?

### Recommended readable command

```powershell
Get-Location
```

Anatomy:

```text
Get-Location = command name
No argument  = current PowerShell location inspect karo
```

### Path value clearly select karna

```powershell
Get-Location | Select-Object -ExpandProperty Path
```

Is command mein pipeline (`|`) first command ka object next command ko deti hai.
Pipeline detail later aayegi; abhi result sirf clean path evidence hai.

### `pwd` shorthand

```powershell
pwd
```

PowerShell mein `pwd`, `Get-Location` ka alias hai. Interactive use mein short hai,
lekin learning notes/scripts mein `Get-Location` intention zyada clearly batata hai.

Alias verify kar sakte hain:

```powershell
Get-Alias -Name pwd
```

## Prompt aur CWD

Terminal prompt commonly current location show kar sakta hai:

```text
PS C:\Users\ajaym\Desktop\Practicle>
```

Lekin prompt customize ho sakta hai. Isliye exact evidence chahiye to prompt ko guess
karne ke bajay `Get-Location` run karo.

## Current location badalna

PowerShell command:

```powershell
Set-Location -Path notes
```

Common alias:

```powershell
cd notes
```

Yeh shell ki current location change karta hai; folder ko move/rename/delete nahi
karta. Is lesson ki verification mein location intentionally change nahi ki gayi,
taaki workspace context stable rahe. Folder navigation ko path concepts ke saath next
topics mein safely practice karenge.

## Har process ka apna working context ho sakta hai

CWD computer ki ek universal global location nahi hai. Alag terminal/shell processes
different directories mein ho sakte hain:

```text
PowerShell window A -> C:\Projects\TaskForge
PowerShell window B -> C:\Downloads
VS Code terminal    -> workspace-selected directory
```

Ek terminal mein location change karne se doosre independent terminal ki location
automatically change hona zaroori nahi.

## Child process aur working directory

Jab shell `node`, `npm` ya kisi aur program ko start karega, woh program normally shell
ke working-directory context se start hota hai. Isi wajah se same command wrong folder
se run karne par config ya project file nahi mil sakti.

```text
PowerShell CWD
  -> program start
  -> program ko starting working directory milti hai
  -> relative file lookup isi context par depend kar sakta hai
```

Program baad mein apni working directory change kar sakta hai, isliye CWD ko process
context ki property samjho.

## Project root aur CWD same hona kab useful hai?

Future mein `taskforge-backend/` project root hoga. Project commands commonly project
root se run karna easiest/reliable hota hai because wahi `package.json`, source folders
aur configuration expected locations par honge.

```text
taskforge-backend/       <- future project root and common command CWD
  package.json
  src/
  tests/
  notes/
```

Folder Topic 33 se pehle create nahi karenge.

## Wrong CWD se common problems

### File/path not found

```text
Cause: command relative target ko wrong base folder mein search kar rahi hai.
Check: Get-Location
Fix: intended directory/target verify karo; guessing mat karo.
```

### `package.json` not found

Future npm command wrong folder se run ho to npm ko project manifest nahi mil sakta.
Pehla diagnostic question hoga: **command run karte waqt CWD kya thi?**

### Wrong project modify ho jana

Same-name folders/files multiple locations par ho sakte hain. State-changing command
se pehle `Get-Location` aur exact target inspect karna accidental modification se
bachata hai.

### Prompt dekhkar incorrect assumption

Custom prompt location hide ya shorten kar sakta hai. Fix: `Get-Location` evidence lo.

## Safe debugging sequence

Jab command kahe ki file/config nahi mili:

```text
1. Error ko poora read karo
2. Get-Location run karo
3. Expected project root identify karo
4. Read-only listing se expected file inspect karo
5. Target confirm hone ke baad hi location/action change karo
```

`cd` repeatedly guess karke chalana debugging method nahi hai.

## TaskForge data/execution flow

```text
Developer terminal kholta hai
  -> PowerShell ki ek CWD hoti hai
  -> developer project command run karta hai
  -> shell program ko CWD context mein start karta hai
  -> program config/source relative location se find kar sakta hai
  -> output/error terminal par return hota hai
```

Yeh HTTP request data flow nahi; local development command execution flow hai.

## Verified evidence

Read-only inspection:

```powershell
Get-Location | Select-Object Path
```

Result:

```text
Path
----
C:\Users\ajaym\Desktop\Practicle
```

Additional consistency check:

```powershell
(Get-Location).Path
```

Dono inspections same location return karni chahiye. Verification ne directory change,
file creation, file modification ya deletion nahi ki.

## Student exercise

Apne terminal mein yeh read-only steps karo:

1. `Get-Location` run karo.
2. Output mein directory identify karo.
3. `pwd` run karo aur compare karo.
4. `Get-Alias -Name pwd` se alias target identify karo.
5. Explain karo ki `Get-ChildItem -Path notes` mein `notes` kis base se resolve hoga.

## Exercise answer

Current project session ke liye expected concepts:

```text
Get-Location result: C:\Users\ajaym\Desktop\Practicle
pwd: Get-Location ka alias
pwd aur Get-Location: same current location show karte hain
notes ka base: current working directory
resolved target conceptually: C:\Users\ajaym\Desktop\Practicle\notes
```

Machine/session badalne par actual CWD different ho sakti hai, isliye hard-coded answer
yaad karne ke bajay inspect karna zaroori hai.

## Interview question with Hinglish answer

**Question:** Current working directory kya hoti hai aur backend development mein kyun
important hai?

**Answer:** Current working directory, ya CWD, woh directory context hai jahan se shell
ya process kaam kar raha hota hai. Relative paths isi base ke against resolve ho sakte
hain. Backend commands wrong CWD se run karne par `package.json`, config ya source file
not-found errors aa sakte hain. PowerShell mein main `Get-Location` se CWD verify karta
hoon aur state-changing command se pehle intended project root confirm karta hoon.

## Easy-English minimum interview answer

**The current working directory is the folder context used by a shell or process.
Relative paths are resolved from this location, so I check it with `Get-Location`
before running project commands.**

Short version:

**The current working directory is the folder where a command runs. In PowerShell, I
check it with `Get-Location`.**

## Completion boundary

Topic 20 mein CWD definition, inspection, process context, TaskForge relevance,
debugging flow aur safety complete hue. **Topic 21 — Absolute aur relative paths** next
hai aur abhi start nahi hua.

