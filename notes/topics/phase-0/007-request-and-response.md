# Topic 7 — Request aur response kya hain?

## Aaj ka exact objective

Aaj client-server communication ke do basic messages samajhne hain:
**request** aur **response**. HTTP method, URL, headers, body aur status codes Phase 6
mein detail se padhenge. Aaj server ya endpoint code create nahi hoga.

## Prerequisite recap

- Client communication initiate karke service/operation maangta hai.
- Server communication accept karke processing aur result provide karta hai.
- API available operations aur expectations ki boundary/contract hoti hai.

Ab is interaction mein client jo bhejta hai aur server jo lautata hai, unhe basic
level par request aur response kahenge.

## Request ki simple definition

**Request client se server ko bheji gayi communication hoti hai jisme client kisi
information, resource ya operation ki demand karta hai aur zarurat ke according
supporting information deta hai.**

Request ke conceptual parts:

- **Intent:** client kya karwana chahta hai?
- **Target:** kis resource/entity par kaam chahiye?
- **Input:** operation ke liye kya information chahiye?
- **Context:** identity/permission jaise additional context ki zarurat ho sakti hai.

Exact technical structure baad mein aayega.

## Response ki simple definition

**Response server se client ko diya gaya result hota hai jo batata hai ki requested
operation ka kya outcome hua aur relevant result ya safe error information deta hai.**

Response sirf successful data nahi hoti. Valid rejection ya controlled error bhi
response hai.

## Basic round trip

```text
CLIENT                                  SERVER
   |                                       |
   | REQUEST: "Task create karo"           |
   |-------------------------------------->|
   |                                       | rules/checks/work
   | RESPONSE: success ya safe failure     |
   |<--------------------------------------|
   |                                       |
```

Client se server aur phir server se client tak ke complete interaction ko often
**round trip** kaha jata hai.

## TaskForge task-create example

Conceptual request:

```text
Intent: Create task
Target context: Project Alpha
Input: Title = "Prepare release notes"
User context: Operation kis signed-in user ne initiate ki
```

Possible success response:

```text
Outcome: Success
Result: Created task ki safe details
```

Possible failure response:

```text
Outcome: Failure
Reason: Task title missing, project unavailable, ya user not allowed
```

Yeh conceptual shapes hain, actual JSON/API contract nahi.

## Complete TaskForge flow

```text
User action
  -> frontend/client input collect karta hai
  -> client request banakar API boundary se server ko bhejta hai
  -> server/backend request samajhkar validation aur permission check karta hai
  -> zarurat par database operation coordinate karta hai
  -> server response banata hai
  -> client response interpret karta hai
  -> frontend success/error user ko dikhata hai
```

Reverse result flow samajhna important hai: database directly user ko response nahi
deta; backend result ko safe application response mein translate karta hai.

## Request intent aur input same nahi

```text
Intent: Task create karna
Input: title, priority, project identity
```

Intent operation batata hai. Input operation ke liye supplied values batata hai.

## Response aur screen same nahi

Server response software-readable result hota hai. Frontend us response ko human-
friendly screen/message mein convert kar sakta hai.

Example:

```text
Server response meaning: title required
Frontend display: "Please enter a task title."
```

Response aur visible UI related hain, identical nahi.

## Success response

Success response ka meaning hai server ne requested operation successfully handle
kiya. Result operation par depend karega:

- task create par created task details;
- task list par tasks;
- task update par updated result;
- kuch successful operations mein detailed data ki zarurat nahi ho sakti.

Exact status codes later topics mein aayenge.

## Error/failure response

Server request receive karke controlled failure return kar sakta hai:

- invalid input;
- missing resource;
- permission denied;
- conflict with current state;
- unexpected internal problem.

Failure response bhi correct system behaviour ho sakta hai. Invalid operation ko
success bolna incorrect behaviour hoga.

Safe response ko user/client ke liye useful hona chahiye, lekin secrets, stack trace,
database credentials ya sensitive internal details expose nahi karni chahiye.

## Response aur no response mein difference

### Failure response received

Server ne request receive/process ki aur failure result client ko mil gaya.

```text
Client -> server
Client <- safe error response
```

### No response received

Client ko result hi nahi mila. Possible reasons:

- server reachable nahi;
- communication interrupt;
- server hang/crash;
- client timeout;
- client-side handling problem.

```text
Client -> ? -> server
Client <- no usable result
```

Debugging mein in dono ko same nahi maanna chahiye.

## One request and one response

Traditional HTTP mental model mein ek client request ke liye server ek response
return karta hai. Lekin long-running jobs, streaming aur realtime connections mein
communication patterns more complex ho sakte hain. Woh later topics hain.

Abhi foundational model:

```text
one request -> one corresponding response
```

## Client responsibilities

Client generally:

- correct operation choose karta hai;
- expected format mein input prepare karta hai;
- request send karta hai;
- response receive/interpret karta hai;
- success, failure, loading ya retry state handle karta hai.

## Server responsibilities

Server generally:

- request accept aur interpret karta hai;
- input, identity aur permissions validate karta hai;
- business rules apply karta hai;
- required work coordinate karta hai;
- truthful success/failure response deta hai;
- sensitive internal details hide karta hai.

## Common misconceptions

1. **"Request sirf form submit hoti hai."**  
   Nahi. Data read, update, delete, login aur other operations ke liye bhi request
   ho sakti hai.

2. **"Response matlab successful data."**  
   Safe error/rejection bhi response hoti hai.

3. **"Frontend directly database se response leta hai."**  
   TaskForge design mein backend database result ko application response mein
   translate karega.

4. **"Button click hi request hai."**  
   Click user event hai. Frontend us event ke result mein request create/send kar
   sakta hai.

5. **"Failure response aur no response same hain."**  
   Failure response server ka delivered outcome hai; no response communication ya
   availability issue indicate kar sakta hai.

6. **"Request send hui to operation definitely complete hua."**  
   Request attempt aur confirmed successful response alag facts hain.

## Verification strategy

Topic understood hai agar learner:

1. request aur response define kar sake;
2. directions bata sake: request client-to-server, response server-to-client;
3. intent aur input separate kar sake;
4. success response, failure response aur no response distinguish kar sake;
5. TaskForge operation ka round trip trace kar sake;
6. explain kar sake ki button click aur network request identical kyun nahi.

## Quick self-check

1. Request kya hoti hai aur kaun bhejta hai?
2. Response kya hoti hai aur kaun deta hai?
3. Error result response ho sakta hai?
4. Failure response aur no response mein kya difference hai?
5. Task create request ka intent aur input kya ho sakte hain?
6. Server ko response mein stack trace expose karni chahiye?

## Practice exercise

TaskForge **task list** operation ke liye fill karo:

```text
Client:
Request intent:
Possible request input/context:
Server processing:
Success response:
Failure response:
No-response situation:
Frontend display:
```

Pehle khud attempt karo, phir sample dekho.

<details>
<summary>Answer-after-attempt</summary>

```text
Client: TaskForge web frontend
Request intent: Project ke tasks read karna
Possible input/context: Project identity aur current user identity
Server processing: Input, access aur rules check karke tasks obtain karna
Success response: Allowed tasks ki safe list
Failure response: Project missing ya access denied result
No-response situation: Server unreachable hone se result receive na hona
Frontend display: Task list, safe error message, ya retry state
```

</details>

## Interview question with Hinglish answer

**Question:** Request aur response kya hote hain?

**Answer:** Request client se server ko bheji gayi communication hoti hai jisme
client information ya operation maangta aur relevant input deta hai. Response server
ka returned outcome hota hai, jo success result ya safe failure information ho sakta
hai. TaskForge mein client task-create request bhejega, server rules process karega aur
created task ya controlled error response dega.

## Easy-English minimum interview answer

**A request is a message sent by a client to ask a server for data or an operation.
A response is the result returned by the server, which can contain success data or
error information. In TaskForge, the client can send a request to create a task, and
the server returns a success or failure response.**

### Even shorter version

**A client sends a request, and the server processes it and returns a response.**

## Topic boundary

Topic 7 mein request/response ka basic mental model complete hua. **Runtime kya
hota hai?** Topic 8 ko iske baad separately complete kiya gaya.
