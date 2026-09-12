# Topic 24 — Source file aur configuration file

## Learning goal

Aaj files ko sirf extension se nahi, **responsibility** se classify karna seekhenge.
Source file application logic/instructions contain karti hai; configuration file
application ya tool ka behavior values/options se control karti hai. Documentation,
data, secrets, generated files aur dependencies ko bhi separately identify karenge.

File extensions ka detailed meaning Topic 25 aur hidden files Topic 26 mein aayenge.

## Source file kya hoti hai?

**Source file developer-written instructions/logic contain karti hai jise runtime,
compiler, interpreter ya tool application behavior banane ke liye process karta hai.**

Future TaskForge examples:

```text
src/server.js
src/routes/task.routes.js
src/controllers/task.controller.js
src/models/task.model.js
```

Source code decide kar sakta hai:

- request kaise handle hogi;
- input validation kaise hogi;
- business rule kya hai;
- database operation kab chalega;
- response/error kaise return hoga.

Hypothetical source example—actual file abhi create nahi hui:

```js
// Yeh function do numbers ko add karke result return karta hai.
function add(firstNumber, secondNumber) {
  // Yeh calculated sum caller ko wapas deta hai.
  return firstNumber + secondNumber;
}
```

Function ka algorithm/logic change karne se program behavior change hota hai.

## Configuration file kya hoti hai?

**Configuration file application ya development tool ko options/values deti hai jinke
through same code different environment ya policy ke according behave kar sakta hai.**

Future examples:

```text
package.json          -> npm/project manifest and scripts/dependency declarations
.env                  -> local environment values/secrets; Git mein commit nahi
.env.example          -> required variable names ka safe template; no real secrets
eslint configuration  -> linting rules
CI workflow           -> automation instructions/configuration
```

Hypothetical JSON configuration example:

```json
{
  "name": "taskforge-backend",
  "type": "module"
}
```

Yeh current repository ka real `package.json` nahi. JSON comments support nahi karta,
isliye teaching explanation code block ke outside hai.

## Core difference

```text
Source code     -> application kaise kaam karegi?
Configuration   -> application/tool kis value, option ya environment ke saath chalega?
```

Example:

```text
Source logic: server configured port par listen kare
Configuration value: PORT=3000
```

Port `3000` ko source ke many places mein hard-code karne ke bajay configuration se
provide karna environments ko flexible banata hai.

## Comparison

| Question | Source file | Configuration file |
|---|---|---|
| Main responsibility | Logic/instructions | Options, values, policy or tool setup |
| Typical consumer | Runtime/application/build tool | Application or development tool |
| Change effect | Algorithm/behavior flow बदल सकता है | Existing behavior ka mode/value बदल सकता है |
| Example | `src/server.js` | `package.json`, `.env` |
| Secret rakhna? | No | Only appropriate untracked secret mechanism; most config still non-secret |
| Tests needed? | Usually yes | Validation/integration check often needed |

Boundary perfect/universal nahi. Kuch configuration JavaScript file mein ho sakti hai,
aur CI configuration executable behavior describe kar sakti hai. Classification file
extension se nahi, primary responsibility aur consumer se karo.

## Source aur configuration together kaise kaam karte hain?

```text
Configuration source/value
  -> application startup
  -> source code configuration read karta hai
  -> value parse/validate hoti hai
  -> valid typed setting application logic ko milti hai
  -> application behavior execute hota hai
```

Future conceptual JavaScript—actual project code nahi:

```js
// Yeh environment se PORT ki raw text value read karta hai.
const rawPort = process.env.PORT;

// Yeh raw text ko number mein convert karta hai.
const port = Number(rawPort);

// Yeh invalid port ko startup par reject karta hai.
if (!Number.isInteger(port) || port <= 0) {
  // Yeh clear configuration error ke saath process startup rokta hai.
  throw new Error("PORT must be a positive integer");
}
```

Important: environment variables strings ke roop mein mil sakti hain. Source code ko
parse aur validate karna hota hai; configuration ko blindly trust nahi karna.

## Hard-coded value kya hoti hai?

Hard-coded value directly source logic mein fixed hoti hai:

```js
// Yeh port ko source code mein permanently fixed kar raha hai.
const port = 3000;
```

Har constant configuration nahi hota. Stable business/domain constant source mein valid
ho sakta hai. Environment-dependent value—port, database URL, external service key—ko
configuration mechanism se lena commonly better hota hai.

Question pucho:

```text
Kya value environment/deployment/user policy ke saath badalni chahiye?
```

If yes, configuration candidate hai. But unlimited configurability bhi complexity badha
sakti hai; real variation justify honi chahiye.

## Configuration ke common sources

Application configuration sirf “config file” se nahi aati:

```text
Default values in source
  -> configuration file
  -> environment variables
  -> command-line arguments
  -> secret manager/deployment platform
```

Precedence policy explicit honi chahiye: same key multiple sources mein ho to kaunsa
win karega? TaskForge configuration module later ek predictable rule define karega.

## Secrets aur configuration

Secret ek sensitive configuration value ho sakta hai:

```text
Database password
JWT signing secret
Email provider API key
```

Lekin har configuration secret nahi:

```text
Port number
Log level
Public application name
Pagination default
```

Rules:

- real secrets source file mein hard-code nahi;
- secrets Git mein commit nahi;
- `.env.example` mein real value nahi, only safe placeholder/name;
- logs/errors/terminal screenshots mein secret print nahi;
- production secrets dedicated environment/secret management se;
- accidental exposure ho to secret rotate karna padta hai—sirf file delete enough nahi.

## Documentation file

Current repository ki `.md` files learning/product documentation hain:

```text
notes/BACKEND_ROADMAP.md
notes/ARCHITECTURE.md
LEARNING_MEMORY.md
```

Documentation humans ko intent, design, usage aur progress explain karti hai. Runtime
normally in Markdown lessons ko TaskForge application logic ki tarah execute nahi karega.

Current verified inventory:

```text
Markdown files: 38
JavaScript files: 0
package.json: absent
```

So current repository state documentation-only hai; application source/config abhi
initialize nahi hui.

## Data file

Data file application/tool ka input/output data store kar sakti hai, jaise seed fixture
ya export. Example:

```text
tests/fixtures/users.json
```

JSON extension dekhkar automatically configuration assume nahi karna. Agar file test
users ka dataset contain karti hai, primary responsibility data hai.

## Test file

Test file executable source code ka special category ho sakti hai jo expected behavior
verify karti hai:

```text
tests/task.test.js
```

Test application production feature implement nahi karti; feature behavior assert karti
hai. Tests future testing phase mein create honge.

## Generated file

Generated file tool/build/process banata hai. Developer ko directly edit nahi karna
chahiye if next generation overwrite karegi.

Examples could include:

```text
coverage reports
build output
lockfile (tool-managed but reviewed/committed according to project policy)
```

Generated ka meaning disposable always nahi. `package-lock.json` important reproducible
dependency record ho sakta hai. Tool ownership aur project policy samjho.

## Dependency/vendor files

Installed dependency files project source ke consumers/supporting code hote hain, but
normally application team unhe directly edit nahi karti. Future `node_modules/` ko
source-control mein commit nahi karenge; manifest/lockfile se recreate karenge.

## Manifest kya hota hai?

Manifest project/package metadata and declared relationships describe karta hai.
Future `package.json` configuration/manifest role निभाएगा:

```text
project name
module type
scripts
dependencies
development dependencies
```

It can cause tools to run commands, so config file harmless text assume nahi. Untrusted
repository scripts inspect karke hi execute karo.

## Configuration validation kyun zaroori hai?

Config external input hai. Problems:

- required value missing;
- number ki jagah invalid text;
- malformed URL;
- unsupported enum, e.g. wrong log level;
- production mein unsafe default;
- secret too short/empty.

Best startup flow:

```text
read -> parse -> validate -> normalize -> expose safe config object
```

Fail fast ka meaning: invalid critical configuration detect hote hi clear error ke saath
startup stop ho, later obscure runtime failure nahi.

## Configuration change kab apply hoti hai?

File save karne se running process automatically new config use kare, guaranteed nahi.
Common application startup par configuration once read karti hai; change ke baad process
restart required ho sakta hai. Kuch systems dynamic reload implement karte hain.

```text
Config file changed on disk
  != running process state automatically changed
```

Source file changes bhi running process ko automatically reload tabhi karengi jab dev
watcher/hot-reload tool configured ho. Source code aur running process Topic 9 ka same
distinction yahan apply hota hai.

## Environment-specific configuration

```text
Development -> local database, verbose logs
Test        -> isolated test database/config
Production  -> production services, safer logging/security settings
```

Same source code ideally environment values se adapt kare. Complete configuration files
duplicate karke drift create karne ke bajay shared schema + environment values useful
ho sakte hain.

## Common mistakes aur fixes

### Filename/extension se responsibility guess

Fix: content ka purpose, owner aur consumer identify karo. `.js` config bhi ho sakti hai;
`.json` data ya manifest bhi.

### Secret source/config repository mein commit

Fix: tracking se exclude, exposed secret rotate, history/exposure scope inspect, safe
example template maintain.

### Configuration blindly use

Fix: startup boundary par parse and validate.

### Same config key many files mein

Fix: clear source of truth and precedence define; duplication reduce.

### Environment-specific value hard-code

Fix: justified value configuration boundary mein move.

### Everything configurable banana

Fix: only real environment/product variation expose; unnecessary switches maintainability
hurt karte hain.

### Generated/dependency file manually edit

Fix: owning tool/source update karke regenerate; project policy check.

### Config save but process old behavior

Fix: determine config kab read hoti hai and whether restart/reload required.

## TaskForge planned responsibility map

```text
taskforge-backend/
  src/                 -> application source logic
  tests/               -> verification source
  package.json         -> npm project manifest/configuration
  package-lock.json    -> npm-managed dependency resolution record
  .env                 -> local secret/environment values; untracked
  .env.example         -> safe required-variable template
  README.md            -> project documentation
```

Yeh planned conceptual structure hai, current filesystem output nahi. Actual project
root Topic 33 mein create hoga.

## How to classify an unfamiliar file

```text
1. Exact path/name inspect karo
2. Content safely read karo; secrets print nahi
3. File ko kaun read/execute/generate karta hai?
4. Primary responsibility: logic, configuration, data, docs, test or generated?
5. Change karne se kya behavior/state affect hoga?
6. Tracked, generated or secret policy kya hai?
7. Relevant validation/test kaunsa run hoga?
```

## Student exercise

Har planned file ko primary category do aur reason explain karo:

```text
src/server.js
package.json
.env
.env.example
README.md
tests/task.test.js
tests/fixtures/users.json
package-lock.json
```

Extra question: `eslint.config.js` ki extension `.js` hai. Kya woh application source
feature hai ya tooling configuration? Filename nahi, responsibility se answer do.

## Exercise answer

```text
src/server.js             -> application source logic
package.json              -> manifest/configuration
.env                      -> local environment configuration/secrets
.env.example              -> safe configuration-variable template
README.md                 -> documentation
tests/task.test.js         -> test source
tests/fixtures/users.json  -> test data
package-lock.json          -> tool-managed dependency resolution record
eslint.config.js          -> tooling configuration expressed in JavaScript
```

Some files multiple roles touch kar sakti hain; answer primary responsibility aur
consumer ke reason ke saath defend karo.

## Interview question with Hinglish answer

**Question:** Source file aur configuration file mein kya difference hai?

**Answer:** Source file application ki instructions, algorithms aur business logic
contain karti hai. Configuration file application ya tool ko environment-dependent
values, options ya policies deti hai. Main classification extension se nahi, primary
responsibility aur consumer se karta hoon. Configuration external input hai, isliye
startup par parse/validate karta hoon aur secrets source/Git mein hard-code nahi karta.

## Easy-English minimum interview answer

**A source file contains application logic and instructions. A configuration file
provides values or options that control how the application or a tool behaves. I
validate configuration and never hard-code secrets in source code.**

Short version:

**Source files define the logic. Configuration files provide settings used by that
logic or by development tools.**

## Completion boundary

Topic 24 mein source/config responsibility, supporting file categories, validation,
secrets, runtime reload and TaskForge planned mapping complete hua. **Topic 25 — File
extension ka meaning** next hai aur abhi start nahi hua.

