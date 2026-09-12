# Topic 32 — Port aur process ka basic introduction

## Learning goal

Process, PID, port, IP address, protocol, listening socket, client connection and server
lifecycle ka beginner mental model banana hai. Port conflict, reachability, security and
safe inspection/cleanup bhi samajhna hai.

## Process kya hota hai?

**Process running program ka active instance hota hai.**

```text
Program/file on disk
  -> operating system starts it
  -> process gets memory/resources/PID
  -> process runs
  -> process exits and resources release
```

Same program ke multiple processes ho sakte hain. Example, multiple PowerShell terminals
each separate `pwsh` process ho sakte hain.

## PID kya hai?

PID—process identifier—operating system dwara running process ko diya numeric identity
hai.

```powershell
Get-Process -Id $PID
```

`$PID` current PowerShell process ka ID hai. PID permanent identity nahi; process exit ke
baad OS number reuse kar sakta hai. Logs/debugging mein PID ko timestamp/process name ke
context ke saath read karo.

## Source code, program aur process

```text
server.js         -> source file
node.exe          -> runtime program/executable
node server.js    -> running Node process
PID 1234          -> one process instance identity
```

Source edit karne se running process automatically update only when watcher/reload
mechanism exists. Otherwise restart required.

## Foreground aur background process

- **Foreground:** current terminal attached; input/output directly interact kar sakta hai.
- **Background:** terminal ko prompt/control return ho sakta hai while process continues.

Terminal close/Ctrl+C ke baad process definitely stop ho, universal guarantee nahi.
Launch method, parent-child relationship and OS signals matter.

## Port kya hota hai?

**Port number operating system ke network endpoint par specific application/service ko
identify karne mein help karta hai.**

Ek machine/IP par many services ho sakti hain:

```text
IP address -> machine/network interface
Port       -> us interface par service endpoint
```

Example future URL:

```text
http://localhost:3000
```

```text
http      = protocol scheme
localhost = host name
3000      = port
```

Port physical socket/hole nahi; networking number/endpoint component hai.

## Port range

TCP/UDP port numbers `0` through `65535` field range use karte hain. Beginner grouping:

```text
0           -> normal service port ke roop mein special/reserved; bind request mein OS
               se available ephemeral port choose karwane ke liye use ho sakta hai
1–1023      -> well-known/system service range
1024–49151  -> registered/user range
49152–65535 -> dynamic/private/ephemeral range
```

Privileges/policies and actual availability OS/environment-specific hain. “High port”
automatically safe/free nahi.

## IP + protocol + port = endpoint context

Port number alone full identity nahi:

```text
TCP + 127.0.0.1 + 3000
UDP + 127.0.0.1 + 3000
TCP + 0.0.0.0 + 3000
```

These differ by transport/address binding. OS rules determine simultaneous bindings.

## TCP versus UDP basic

- **TCP:** connection-oriented, ordered reliable byte stream; HTTP commonly TCP use
  karta hai (modern HTTP transports may differ).
- **UDP:** connectionless datagrams; delivery/order guarantee TCP jaisi nahi.

TaskForge HTTP server ke beginner mental model mein TCP listener use karenge. Detailed
network protocols later.

## Listen ka meaning

Server process address/port par socket bind karke incoming connections ke liye listen
karta hai:

```text
Server process
  -> bind TCP 127.0.0.1:3000
  -> listen
  -> client connects
  -> server accepts connection
  -> data exchange
  -> connection closes
```

Process running hona port listening prove nahi. Port listening hona HTTP route healthy
prove nahi.

## Client aur server connection

Client connection mein both endpoints hote hain:

```text
client local endpoint: temporary client port
server remote endpoint: known listening port
```

Verified experiment:

```text
Server listener: 127.0.0.1:63787
Client local:     127.0.0.1:63788 (IPv4-mapped display)
Client remote:    127.0.0.1:63787
```

Client port OS selected temporary port tha. Future run mein numbers different honge.

## Loopback, localhost and all interfaces

```text
127.0.0.1 / localhost -> local machine loopback access
0.0.0.0               -> commonly all available IPv4 interfaces par bind intent
```

Development server loopback bind external network exposure reduce karta hai. `0.0.0.0`
LAN/container access ke liye useful ho sakta hai but security/firewall/access implications
samajhni hongi.

Binding address alone public internet exposure prove nahi; firewall/router/cloud network
rules additional layers hain.

## Port ownership

Listening socket running process own karta hai. Process exit/socket close hone par port
binding release hoti hai, though networking state/timing nuances exist.

```text
PID -> process -> socket -> local address/port
```

Port ko “kill” nahi karte; owning process/socket lifecycle manage karte hain.

## Port conflict

Same conflicting address/protocol/port already bound ho to new server fail kar sakta hai.
Node commonly error code show karega:

```text
EADDRINUSE
```

Meaning broadly: requested address/port already in use.

Safe diagnosis:

```text
1. exact port/address/error inspect
2. listener and owning PID identify
3. process name/command/ownership verify
4. decide: existing intended server use/stop, or configured port change
5. unrelated process blindly terminate nahi
```

Random higher port choose karna root cause hide kar sakta hai.

## PowerShell inspection commands

Current process:

```powershell
Get-Process -Id $PID
```

Listening connection table conceptually:

```powershell
Get-NetTCPConnection -State Listen
```

Exact port:

```powershell
Get-NetTCPConnection -LocalPort 3000 -State Listen
```

These commands permissions/environment ke cause fail kar sakte hain. Alternative Windows
tools exist, but output mapping and permissions still verify karne hote hain.

## Observed permission error

Temporary listener ke first verification attempt mein:

```text
Get-NetTCPConnection: Access denied
```

Meaning OS connection-table query current context mein permitted nahi thi. Listener
creation failure nahi. `finally` cleanup ran, so temporary listener stop hua. Permission
bypass/admin escalation nahi ki.

Smallest safe fallback: same process mein loopback client created, connection established,
server accepted it, then both sockets dispose and listener stop verify hua.

## Verified temporary experiment

Fixed port conflict avoid karne ke liye port `0` request kiya; OS ne free temporary port
assign kiya.

```text
ProcessName=pwsh
OwningPID=20940
ListenerEndpoint=127.0.0.1:63787
ClientConnected=True
ServerAccepted=True
ClientLocalEndpoint=127.0.0.1:63788
ClientRemoteEndpoint=127.0.0.1:63787
ListenerActiveAfterStop=False
```

PID/ports one verification run ke ephemeral evidence hain; future run values change
hongi. No permanent/background service remains.

## Process lifecycle

```text
Created
  -> starting
  -> running
  -> optionally listening
  -> serving/work
  -> shutdown signal/request
  -> stop accepting new work
  -> finish/abort active work according to policy
  -> close resources
  -> exited
```

Not every process server/listener hota hai. Short command runs then exits; backend server
long-running process hota hai.

## Graceful shutdown

Graceful shutdown means process stop hote waqt controlled cleanup attempt:

- new requests stop/limit;
- active work finish policy;
- database connections close;
- network server close;
- logs flush;
- exit status return.

Force termination data loss/incomplete work cause kar sakti hai. Exact signal handling
Node phase mein implement hoga.

## Ctrl+C

Foreground terminal process ko Ctrl+C interrupt signal request bhej sakta hai. Process
handle/ignore/delay kar sakta hai; OS and runtime semantics matter. Repeated forceful
termination se pehle output and shutdown state inspect.

## Port configuration

Future TaskForge:

```text
PORT environment value
  -> read as text
  -> parse integer
  -> validate range/policy
  -> server listen
  -> actual bound endpoint log safely
```

Port hard-code many files mein nahi. Development default possible, production environment
override possible. `PORT` missing/invalid behavior explicit hoga.

## Port available check race

“Check free then later bind” ke beech another process port claim kar sakta hai:

```text
check says free
  -> time gap
  -> another process binds
  -> our bind fails
```

This is a race condition. Actual bind result authoritative hai; `EADDRINUSE` handle/report
karna necessary.

## Process environment isolation

Each process apne PID, memory, environment snapshot, CWD and open resources rakhta hai.
One terminal environment variable change already-running server ko automatically update
nahi karti. Restart/new process required commonly.

## Common errors aur fixes

### EADDRINUSE

Owning listener/PID verify; intended duplicate server stop or configuration change.

### Connection refused

Host reachable ho sakta but target endpoint par listener absent/not accepting. Server
process/listen output/address/port inspect.

### Timeout

Network route/firewall/server overload/hang possible. “Server absent” only conclusion
nahi.

### Server running but route fails

Process + port are lower layers. HTTP method/path/auth/business/database error separately
inspect.

### Works on localhost, not another device

Loopback-only binding, firewall/network rules or wrong host. Security assess kiye bina
all-interfaces expose nahi.

### Orphan/stale process

Old server still running after new terminal. Owning PID/command/start time verify before
stopping.

### Access denied inspecting ports

Permission/tool limitation. Least-privilege alternate evidence use or only justified
authority request; do not assume no listener.

### Killing wrong process

PID can be reused and same program multiple instances. PID + name + path/command + start
time + port association verify.

## Safety checklist before stopping a process

```text
1. exact PID
2. process name/executable
3. start time/owner if available
4. associated endpoint
5. current task/user ownership
6. data-loss/active-work impact
7. graceful stop method
8. after-state verification
```

System/database/unknown process blindly terminate nahi.

## TaskForge request-level preview

```text
Client sends HTTP request to localhost:PORT
  -> OS routes TCP connection to listening Node process
  -> Express later matches method/path
  -> handler executes
  -> response returns
```

Port identifies network service endpoint; API route identifies resource/action inside
HTTP application. `3000` and `/api/tasks` same concept nahi.

## Student exercise

Without starting permanent server:

1. current shell process name/PID inspect;
2. process and program difference explain;
3. port and API route difference explain;
4. loopback vs `0.0.0.0` explain;
5. `EADDRINUSE` diagnosis order write;
6. graceful shutdown ka reason explain;
7. verified experiment values ko ephemeral kyun label kiya, explain.

## Exercise answer

```text
process = running program instance; PID = temporary OS identity
port = network endpoint component; API route = HTTP application path
127.0.0.1 = local loopback; 0.0.0.0 = all IPv4 interfaces bind intent
EADDRINUSE = conflicting bind; owner verify before stop/change
graceful shutdown closes active work/resources predictably
PID and OS-selected ports future runs mein change ho sakte hain
```

## Interview question with Hinglish answer

**Question:** Process aur port kya hote hain, aur `EADDRINUSE` kaise debug karte ho?

**Answer:** Process running program ka instance hai jiska PID hota hai. Port network
endpoint par service ko identify karta hai; complete binding mein protocol and address bhi
matter karte hain. Server process socket bind/listen karta hai. `EADDRINUSE` par exact
address/port, listening socket and owning PID verify karta hoon, process identity/ownership
confirm karta hoon, phir intended old server gracefully stop ya configured port change
karta hoon. Unknown process blindly kill nahi karta.

## Easy-English minimum interview answer

**A process is a running instance of a program and has a process ID. A port identifies a
network service endpoint. If a port is already in use, I identify the listening process
before stopping it or changing the application's configured port.**

Short version:

**A process runs the program, and a listening socket connects that process to an address
and port.**

## Completion boundary

Topic 32 mein process/PID, port/endpoints, listener/client lifecycle, conflicts,
reachability, security and shutdown complete hue. **Topic 33 — Project root folder create
karna** next hai aur abhi start nahi hua.

