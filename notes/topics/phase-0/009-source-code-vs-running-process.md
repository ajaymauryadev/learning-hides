# Topic 9 — Source code aur running process ka difference

## Aaj ka exact objective

Aaj stored **source code** aur actively executing **running process** ko clearly
separate karna hai. Local development aur production ka difference Topic 10 mein
aayega. Abhi TaskForge application file create ya process start nahi karenge.

## Prerequisite recap

Runtime code execute hone ka environment deta hai. Lekin disk par code file present
hona aur us code ka actively execute hona ek baat nahi hai.

## Source code ki simple definition

**Source code human-readable instructions hoti hain jo files mein stored rehti hain
aur application ka intended behaviour describe karti hain.**

Example future file:

```text
src/server.js
```

File disk par saved ho sakti hai, editor mein open/closed ho sakti hai, copy ki ja
sakti hai aur Git mein track ho sakti hai. Sirf file present hone se server actively
run nahi hota.

## Running process ki simple definition

**Running process kisi program ka active instance hota hai jise operating system
manage karta hai aur jisme runtime code execute kar raha hota hai.**

Process ke paas commonly:

- unique process identifier (PID);
- allocated memory;
- current execution state;
- environment values;
- open files/network resources;
- start time;
- running, waiting ya stopped state.

In internals ko later Node/process topics mein practically inspect karenge.

## Recipe aur chef analogy

```text
Recipe on paper = source code
Kitchen/tools   = runtime environment
Chef cooking    = running process
Prepared food   = behaviour/output
```

Recipe shelf par ho sakti hai, lekin cooking automatically nahi hoti. Ek recipe se
multiple chefs separately cook kar sakte hain—same source se multiple process
instances possible hain.

## Source se process tak conceptual flow

```text
Source-code files disk par
        |
        | start command/runtime ko file dena
        v
Operating system process create karta hai
        |
        v
Runtime instructions execute karta hai
        |
        v
Process memory/resources use karke behaviour deta hai
        |
        v
Process stop/crash hota hai
```

Process end hone par source-code files normally disk par remain karti hain.

## Main differences

| Source code | Running process |
|---|---|
| Disk par stored instructions | Program ka active instance |
| Static file content | Time ke saath changing execution state |
| Editor mein edit hota hai | Operating system/runtime manage karta hai |
| PID nahi hota | Usually PID hota hai |
| Khud CPU work nahi karta | Execute hote waqt CPU/memory/resources use karta hai |
| Process crash se normally delete nahi hota | Crash/stop par instance end ho jata hai |
| Same file se multiple processes start ho sakte hain | Har instance ki separate memory/state ho sakti hai |

## TaskForge example

Future mein:

```text
src/server.js                  = source-code file
node src/server.js start karna = runtime ko instruction
active Node instance           = running TaskForge process
```

File explorer mein `server.js` dikhna prove nahi karta ki TaskForge server process
running hai. Running process ko process/port/log/health checks se verify karna hoga;
yeh later practical topics hain.

## File save karna process restart karna nahi

Imagine running process ne startup par source code load kiya. Aap editor mein source
file change aur save karte ho:

```text
Disk file             = new code
Already-running process = old loaded behaviour continue kar sakta hai
```

New code use karne ke liye process ko restart/reload karna pad sakta hai.

Development tools automatic restart/watch behaviour provide kar sakte hain, lekin
woh extra tool behaviour hai—file save ki universal guarantee nahi.

## Stale process kya hota hai?

Beginner debugging scenario:

```text
1. Old server process running hai.
2. Source code change hua.
3. Expected restart nahi hua.
4. Test old behaviour dikhata hai.
```

Developer soch sakta hai code change work nahi kar raha, jabki request stale/old
process handle kar raha hai. Isliye verify karna hota hai:

- kaunsa process running hai;
- process kab start hua;
- correct file/path se start hua;
- change ke baad restart/reload hua;
- expected port/process use ho raha hai.

Stale server-process debugging Topic 237 mein practically aayegi.

## Same source, multiple processes

```text
                -> Process A (own PID/memory)
Same source code
                -> Process B (own PID/memory)
```

Dono same instructions se start ho sakte hain, lekin inki in-memory state aur
environment different ho sakte hain.

Example: ek process development configuration se aur doosra test configuration se
start ho sakta hai. Environment details later topics mein aayengi.

## Process memory aur persistent database

Running process ki memory temporary hoti hai:

```text
Process starts -> memory allocated -> process runs -> process ends -> memory released
```

Important durable TaskForge data ko sirf process memory par depend nahi karna. Isliye
database persistent data store ke roop mein use hoga.

Process restart hone par in-memory temporary state reset ho sakti hai, jabki properly
stored database data retain reh sakta hai.

## Stop, crash aur delete ka difference

- **Stop:** process intentionally end kiya gaya.
- **Crash:** unexpected error/problem ke karan process end hua.
- **Delete source:** disk se source file remove hui.

Process stop/crash hona source code delete hona nahi hai. Source delete hona running
process ko isi exact moment automatically stop kare, yeh universal assumption bhi
nahi karna chahiye; already-loaded process continue kar sakta hai.

## Start command repeatedly chalane ka risk

Agar old server process already resource/port use kar raha ho aur same server dobara
start karein, new process startup fail kar sakta hai. Later hum `EADDRINUSE` error
padhenge.

Conceptual cause:

```text
Old process -> port use kar raha hai
New process -> same port claim karta hai
Result      -> conflict/startup failure
```

Randomly code rewrite karne se pehle running process state verify karni chahiye.

## Source code, runtime aur process together

```text
Source code = kya instructions execute honi hain
Runtime     = instructions ko execute karne ka environment
Process     = execution ka active operating-system-managed instance
```

Node.js TaskForge context:

```text
JavaScript file -> Node.js runtime -> active Node process
```

## Common misconceptions

1. **"File save ki, server update ho gaya."**  
   Running process ko reload/restart ki zarurat ho sakti hai.

2. **"Terminal band hai to koi process running nahi."**  
   Background/service process possible hai; actual process state verify karni hoti hai.

3. **"Server file exist karti hai, matlab server running hai."**  
   File existence aur active process separate evidence hain.

4. **"Process crash hua to source code lost ho gaya."**  
   Process instance end hota hai; disk files normally remain karti hain.

5. **"Same source code ka sirf ek process ho sakta hai."**  
   Multiple independent instances possible hain.

6. **"Runtime aur process same hain."**  
   Runtime execution environment/software hai; process us program execution ka
   active OS-managed instance hai.

## Success aur failure examples

### Expected success

Correct file Node runtime se start hui, process running hai aur expected behaviour
serve kar raha hai.

### Source problem

File mein invalid instructions hain; runtime process successfully start na kar paye.

### Runtime/environment problem

Required runtime ya capability unavailable hai.

### Process problem

Process crash, stop, hang ya wrong configuration ke saath run kar sakta hai.

### Verification problem

Developer file dekhkar running assume kare, lekin actual process/behaviour check na
kare.

## Verification strategy

Topic understood hai agar learner:

1. source code aur running process separately define kar sake;
2. source -> runtime -> process flow explain kar sake;
3. save aur restart ka difference samjha sake;
4. same source se multiple processes ki possibility explain kare;
5. process memory aur persistent database data distinguish kare;
6. stale process bug ka basic diagnosis bata sake.

## Quick self-check

1. `server.js` file ka exist karna kya server running prove karta hai?
2. Process kya extra state/resources rakhta hai?
3. File edit/save ke baad old behaviour kyun dikh sakta hai?
4. Process crash hone par source code ka kya hota hai?
5. Same source se two processes possible hain?
6. Runtime aur process mein kya difference hai?

## Practice exercise

Scenario:

```text
Aapne TaskForge response text source file mein change kiya.
Test abhi bhi old text dikha raha hai.
```

Answer karo:

```text
Source state:
Possible process state:
First evidence to check:
Smallest likely fix:
Fix verification:
Random code rewrite kyun wrong ho sakta hai:
```

Pehle khud attempt karo, phir sample dekho.

<details>
<summary>Answer-after-attempt</summary>

```text
Source state: New text disk par saved hai
Possible process state: Old code load karke stale process running hai
First evidence: Running process/start time/log aur correct port/path check karo
Smallest likely fix: Confirmed old process ko controlled restart/reload karo
Verification: Same operation repeat karke new response aur process start evidence dekho
Why not rewrite: Problem code logic nahi, old process execution ho sakti hai
```

</details>

## Interview question with Hinglish answer

**Question:** Source code aur running process mein kya difference hai?

**Answer:** Source code disk par stored human-readable instructions hoti hain.
Running process program ka active OS-managed instance hota hai jisme runtime code
execute karta aur memory/resources use karta hai. Same source se multiple processes
start ho sakte hain. File edit/save hone par already-running process automatically
update hona guaranteed nahi, isliye restart/reload aur actual process verification
important hai.

## Easy-English minimum interview answer

**Source code is the set of program instructions stored in files. A running process
is an active instance of that program managed by the operating system, with its own
memory and resources. Saving a source file does not always update an existing
process, so the process may need to be restarted.**

### Even shorter version

**Source code is stored instructions; a process is an active execution of those
instructions.**

## Topic boundary

Topic 9 mein source code aur running process ka difference complete hua. **Local
development aur production ka difference** Topic 10 ko iske baad separately complete
kiya gaya.
