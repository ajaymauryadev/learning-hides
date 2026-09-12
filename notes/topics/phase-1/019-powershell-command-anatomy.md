# Topic 19 — PowerShell command anatomy

## Learning goal

Aaj humein PowerShell command ko dekhkar uske parts identify karne hain. Hum command
run karna hi nahi, balki yeh explain karna seekhenge ki shell ko kaunsa instruction,
target aur option diya gaya. Current working directory aur path rules Topics 20–21
mein detail se aayenge.

## Ek complete example

```powershell
Get-ChildItem -Path notes -Filter "*.md"
```

Is command ko left se right read karo:

```text
Get-ChildItem   -Path       notes       -Filter       "*.md"
command name    parameter   value       parameter     value
```

Meaning: `Get-ChildItem` ko bolo ki `notes` target mein woh items dikhaye jo `*.md`
filter se match karte hain.

## 1. Command name

Command name shell ko batata hai ki **kya action** karna hai.

```powershell
Get-ChildItem
```

PowerShell commands ko cmdlets kaha ja sakta hai. Cmdlet names commonly
`Verb-Noun` pattern follow karte hain:

```text
Get   = action/verb
ChildItem = object/noun
```

Examples:

- `Get-ChildItem`: child items retrieve karo;
- `Get-Content`: file content retrieve karo;
- `Test-Path`: location exist karti hai ya nahi check karo.

Har terminal command cmdlet nahi hoti. PowerShell external programs bhi run kar sakta
hai, jaise future mein `node`, `npm` aur `git`.

## 2. Argument

Argument command ko diya gaya input hota hai. Positional argument apni position se
meaning leta hai.

```powershell
Get-Content README.md
```

Yahan:

```text
Get-Content = command
README.md   = positional argument
```

PowerShell jaanta hai ki `Get-Content` ke is position par file path expected hai.
Beginner ke liye named parameter often clearer hota hai:

```powershell
Get-Content -Path README.md
```

## 3. Named parameter

Named parameter command behavior ya input category ko explicitly name karta hai.
PowerShell mein commonly single hyphen se start hota hai:

```powershell
-Path
-Filter
-File
```

`-Path` batata hai ki agla token target path ki value hai. `-Filter` batata hai ki
agla token filtering rule hai.

## 4. Parameter value

Kuch parameters ko value chahiye:

```powershell
-Path notes
-Filter "*.md"
```

Yahan `notes`, `-Path` ki value hai aur `"*.md"`, `-Filter` ki value hai. Value
parameter ke liye concrete input provide karti hai.

## 5. Switch parameter

Switch ek on/off option jaisa hota hai. Isko separate value dena normally zaroori
nahi hota:

```powershell
Get-ChildItem -File
```

`-File` present hai, isliye only files return karne wala behavior enabled hai.

```text
-Path notes = parameter + value
-File       = switch parameter; separate value nahi
```

## Tokens aur spaces

Shell command line ko meaningful pieces mein parse karta hai. Beginner mental model
mein spaces tokens ko separate karte hain:

```text
Get-ChildItem | -Path | notes | -Filter | "*.md"
```

Lekin quoted text spaces ke baad bhi ek value reh sakta hai:

```powershell
Get-ChildItem -Path "learning notes"
```

`"learning notes"` ek quoted value hai, do independent arguments nahi.

## Quotes kab chahiye?

Spaces ya special characters wali value ko quotes protect/group kar sakte hain:

```powershell
Get-Content -Path "my notes.md"
```

Beginner rule:

- literal text/path ke liye simple value ya quotes use karo;
- spaces hon to quotes lagao;
- copied command ko blindly run mat karo;
- single quote aur double quote ka deeper behavior later cover hoga.

PowerShell mein double-quoted text variables/expressions expand kar sakta hai, jabki
single-quoted text generally literal rehta hai. Abhi secrets ya unknown text ko command
mein interpolate nahi karna hai.

## Alias kya hota hai?

Alias command ka short alternate name hota hai. Example, PowerShell mein `ls` commonly
`Get-ChildItem` ka alias hota hai. Learning notes aur scripts mein full command name
zyada readable hai:

```powershell
Get-Alias -Name ls
```

Alias typing fast banata hai, lekin full `Verb-Noun` name intention clear rakhta hai
aur interview/code review mein explain karna easy hota hai.

## Command ka execution order

```text
1. Developer command line type karta hai
2. Enter press karta hai
3. PowerShell text ko command aur arguments/parameters mein parse karta hai
4. PowerShell command resolve karta hai
5. Parameter values bind hoti hain
6. Command execute hoti hai
7. Result stdout ya diagnostic/error stderr par dikhta hai
8. Exit status success/failure signal de sakta hai
```

PowerShell internally objects bhi pipeline mein pass kar sakta hai. Pipeline ka detail
future topic hai; abhi output ko sirf displayed evidence ki tarah dekho.

## Safe read-only practical commands

### Command discover karo

```powershell
Get-Command -Name Get-ChildItem
```

Anatomy:

```text
Get-Command = command name
-Name       = named parameter
Get-ChildItem = parameter value
```

### Help syntax dekho

```powershell
Get-Help -Name Get-ChildItem
```

Help output mein square brackets usually optional parts represent karte hain. Exact
help notation ko gradually practice karenge.

### Markdown files inspect karo

```powershell
Get-ChildItem -Path notes -Filter "*.md" -File
```

Anatomy:

```text
Get-ChildItem = command
-Path notes   = named parameter and value
-Filter "*.md" = named parameter and value
-File         = switch
```

Yeh read-only command hai: files list karta hai, create/edit/delete nahi.

## Common mistakes aur errors

### Command name typo

```powershell
Get-ChildItems
```

Likely error: term/cmdlet recognize nahi hua. Fix: spelling verify karo aur
`Get-Command -Name Get-ChildItem` use karo.

### Missing parameter value

```powershell
Get-ChildItem -Path
```

PowerShell required argument/value maang sakta hai. Fix: `-Path` ke baad intended
location do.

### Space wali value without quotes

```powershell
Get-Content -Path my notes.md
```

`my` aur `notes.md` separate tokens samjhe ja sakte hain. Fix:

```powershell
Get-Content -Path "my notes.md"
```

### Wrong parameter

Command aisa parameter receive kare jo supported nahi hai to parameter-binding error
aa sakta hai. `Get-Help -Name <CommandName>` se supported syntax inspect karo.

### Dash confusion

Copied rich-text ka long dash `—` PowerShell parameter ke normal hyphen `-` jaisa
nahi hota. Parameters ke liye keyboard wala ASCII hyphen use karo.

## Safety checklist

Command run karne se pehle identify karo:

1. command kya action karegi?
2. target argument/value kya hai?
3. parameters behavior kaise change karenge?
4. command read-only hai ya state change karegi?
5. delete/overwrite/install/publish ho raha hai to exact target aur permission clear hai?

Unknown internet command ko copy-paste karke run nahi karna. Secret, token aur password
command line par expose nahi karna.

## TaskForge connection

Later hum commands dekhenge:

```powershell
npm install express
npm run dev
git status --short
node src/server.js
```

Inmein external program command name hota hai aur remaining tokens us program ke
arguments/options hote hain. Exact npm, Git aur Node behavior unke ordered topics mein
seekhenge; yahan sirf anatomy preview hai.

## Verified evidence

Current repository par safe commands se verify hua:

```text
Get-Command -Name Get-ChildItem
  -> CommandType: Cmdlet
  -> Name: Get-ChildItem

Get-ChildItem -Path notes -Filter "*.md" -File
  -> notes ke matching Markdown files list hue
  -> koi file create, modify ya delete nahi hui
```

## Student exercise

Neeche command ke parts label karo:

```powershell
Get-ChildItem -Path notes -Filter "*.md" -File
```

Phir apne words mein batao:

1. command ka action kya hai?
2. `-Path` aur `-Filter` kya hain?
3. `notes` aur `"*.md"` kya hain?
4. `-File` ko separate value kyun nahi chahiye?
5. command read-only hai ya state-changing?

## Exercise answer

```text
Command name: Get-ChildItem
Named parameter: -Path
-Path value: notes
Named parameter: -Filter
-Filter value: "*.md"
Switch parameter: -File
Action: notes target mein matching Markdown files list karna
Risk type: read-only
```

## Interview question with Hinglish answer

**Question:** PowerShell command ki basic anatomy kya hoti hai?

**Answer:** PowerShell command mein command name action batata hai. Positional argument
apni position se input deta hai. Named parameter, jaise `-Path`, input ya behavior ko
explicitly name karta hai aur uske baad value aa sakti hai. Switch parameter, jaise
`-File`, presence se behavior on karta hai aur normally separate value nahi leta.
PowerShell command ko parse, resolve, parameter-bind aur execute karta hai.

## Easy-English minimum interview answer

**A PowerShell command contains a command name and may contain arguments, named
parameters, parameter values, and switches. The command name defines the action,
while the other parts provide input or change its behavior.**

Short version:

**The command name tells PowerShell what to do. Arguments and parameters tell it what
to use and how to perform the action.**

## Completion boundary

Topic 19 mein command anatomy, parsing flow, common errors aur safety checks complete
hue. **Topic 20 — Current working directory** next hai aur abhi start nahi hua.

