# Topic 15 — Initial development phases

## Aaj ka exact objective

TaskForge ka initial dependency order samajhna hai. Roadmap execute nahi karenge;
sirf samjhenge ki phases is order mein kyun hain aur kya evidence produce karengi.

## Development phase kya hota hai?

**Development phase related learning/implementation work ka bounded group hota hai
jo clear milestone produce karta aur next stage ke prerequisites provide karta hai.**

Phase ek huge code dump nahi. Har numbered topic separately complete hoga.

## Dependency chain

```mermaid
flowchart LR
    P0[Mental model] --> P1[Environment] --> P2[Git] --> P3[JavaScript]
    P3 --> P4[Node.js] --> P5[npm] --> P6[HTTP] --> P7[Express]
    P7 --> P8[Configuration] --> P9[MongoDB] --> P10[Mongoose connection]
    P10 --> P11[Data modelling] --> P12[Schema/model] --> P13[API layers]
    P13 --> P14[First vertical slice]
```

Arrow primary learning dependency dikhata hai—not that previous phase guarantees
every future detail.

## Phase 0 — Mental model/product foundation

Application, frontend/backend, client/server, database, API, runtime, process,
environment, product, users, use cases and system picture.

Milestone: durable product/roadmap/architecture/learning foundation.

Why first: vocabulary unclear ho to commands/code magic lagte hain.

## Phase 1 — Terminal, files and environment

PowerShell, paths, files/folders, VS Code and installed tools verify karenge.

Milestone: known project root and verified development environment.

## Phase 2 — Git foundation

Working tree, staging, commits, branches, remote and secret-safe workflow.

Milestone: inspectable/recoverable repository history.

## Phase 3 — Backend JavaScript

Values, control flow, functions, objects/arrays, errors, async/Promises, JSON and
modules TaskForge examples mein.

Why before framework: Express code JavaScript hai; framework language gaps hide nahi
kar sakta.

## Phase 4 — Node.js

Runtime/process, built-ins, event loop and errors.

Milestone: first TaskForge Node program start/stop/debug.

## Phase 5 — npm

Packages, dependencies, scripts, versions and lockfile.

Milestone: reproducible TaskForge Node workspace.

## Phase 6 — HTTP/networking

Host, port, URL, HTTP messages, methods, headers, body, status and REST.

Milestone: native Node HTTP server and observed request/response.

Why before Express: Express HTTP ko simplify karta hai; underlying contract pehle.

## Phase 7 — Express

Application/server separation, routes, middleware, responses and error flow.

Milestone: verified Express API and liveness endpoint.

## Phase 8 — Configuration/startup

Environment values, secrets, Zod validation, startup order, safe logs and fail-fast
behaviour.

Why before DB connection: credentials/config safely validate honi chahiye.

## Phase 9–10 — MongoDB then Mongoose connection

Pehle database system, access and connection errors; phir Node ODM and connection
lifecycle/readiness.

Milestone: reliable, safely configured database connectivity.

## Phase 11–12 — Modelling then schema/model

Pehle requirements se entities, fields, relationships, ownership and lifecycle;
phir Mongoose schema/model rules.

Why: tool/schema se pehle business-data meaning decide hona chahiye.

## Phase 13 — API architecture layers

Route, middleware, controller, service, model and dependency direction.

Milestone: just-enough feature structure and first vertical-slice design.

## Phase 14 — First vertical slice

One small operation end-to-end connect hoga:

```text
Route -> validation -> controller -> service -> model -> database
      <- safe response/error <- integration test
```

## Vertical slice kya hai?

**Vertical slice ek thin but complete use case hai jo required layers cross karke
observable, testable outcome deta hai.**

Example: one create-workspace operation with validation, rule, persistence, response
and test. Yeh full CRUD/application nahi.

## Phase exit evidence

Scope ke according evidence:

- learner explanation;
- required artifact;
- executed command/output;
- success and important failure checks;
- automated tests where applicable;
- synced learning/architecture/API/debug notes;
- Git diff/history.

Exact Definition of Done Topic 16 mein formalize hogi.

## Advanced tools later kyun?

Docker, Redis, queues, realtime, caching and microservices foundational confusion
solve nahi karte; extra operational problems introduce karte hain.

```text
Simple correct system samjho
  -> real limitation observe karo
  -> justified tool introduce karo
  -> behaviour/tradeoff verify karo
```

## Common misconceptions

1. **"Express se directly start fastest hai."** Short demo possible, independent
   understanding/debugging weak reh sakti hai.
2. **"Planning mein future folders/code bana do."** Empty ceremonial layers current
   truth distort karte hain.
3. **"Phase complete means all topics generated."** Har topic separately verified.
4. **"Schema modelling se pehle likho."** Business relationships pehle decide hon.
5. **"Docker/microservices automatically advanced hain."** Justified decisions matter.
6. **"First vertical slice full CRUD hai."** Woh one thin end-to-end operation hai.

## Verification strategy

Learner ko initial order broadly explain, JavaScript/Node/HTTP before Express justify,
configuration before DB connection, modelling before schema, vertical slice define,
aur advanced tools later rakhne ka reason batana aana chahiye.

## Practice exercise

```text
Mental model -> environment -> ____ -> JavaScript -> ____ -> npm
-> HTTP -> ____ -> configuration -> MongoDB -> ____
-> modelling -> schema/model -> API layers -> ____
```

<details>
<summary>Answer-after-attempt</summary>

```text
Git -> Node.js -> Express -> Mongoose connection -> first vertical slice
```

</details>

## Interview question with Hinglish answer

**Question:** TaskForge ka initial development order kaise plan kiya aur kyun?

**Answer:** Pehle product/system mental model, environment and Git foundation hai.
Phir JavaScript, Node.js, npm and HTTP before Express. Uske baad safe configuration,
MongoDB, Mongoose connection, modelling and first schema/model. Finally API layers
design karke one tested vertical slice banega. Har stage next ki prerequisite aur
verification evidence deta hai.

## Easy-English minimum interview answer

**I planned TaskForge in dependency order: product and tool foundations first, then
JavaScript, Node.js, npm, HTTP, and Express. After that come configuration, MongoDB,
Mongoose, data modelling, and API layers. The first major implementation milestone is
a small tested vertical slice from the API boundary to the database and back.**

### Even shorter version

**The project moves from fundamentals to one tested end-to-end feature, with each
phase providing the prerequisites for the next.**

## Topic boundary

Topic 15 mein initial development phases complete hue. **Definition of Done** Topic
16 ko iske baad separately complete kiya gaya.
