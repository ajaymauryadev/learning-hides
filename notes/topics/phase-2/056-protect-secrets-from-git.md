# Topic 56 — Secrets ko Git se bachana

## Learning goal

Secret kya hai, source code से अलग क्यों रखना है, `.gitignore` की limit क्या है, safe template कैसे
बनती है और secret commit/push होने पर सही incident-response order क्या है—बहुत simple examples से
समझना है।

## Sabse simple definition

**Secret वह private value है जिससे किसी system, account, data या privileged action तक access मिल
सकता है। उसे Git history में commit नहीं करना चाहिए।**

Examples:

```text
database password / connection credential
API key
access token
JWT signing secret
private cryptographic key
email-provider credential
cloud-service credential
session encryption key
```

Non-secret examples:

```text
PORT=3000
NODE_ENV=development
public documentation URL
feature name
```

हर configuration secret नहीं होती, लेकिन unsure हो तो publish करने से पहले owner/security impact
verify करो।

## House-key wala easy example

```text
Source code repository = घर का नक्शा
Secret credential       = घर की चाबी
```

Team को नक्शा मिल सकता है, लेकिन public/shared history में real चाबी attach नहीं करनी चाहिए। अगर
चाबी की photo पहले ही share हो गई, बाद में photo delete करना पर्याप्त नहीं—**lock/key rotate करना**
ज़रूरी है।

## TaskForge practical files

```text
taskforge-backend/
|-- .gitignore
`-- .env.example
```

Relationship:

```text
.env         -> private local values; ignored; commit नहीं
.env.local   -> private/machine values; ignored; commit नहीं
.env.example -> variable names + safe empty/example values; track/commit कर सकते हैं
```

No real `.env` file या credential practical में create नहीं हुआ।

## Safe `.env.example`

Created template:

```dotenv
NODE_ENV=development
PORT=3000
MONGODB_URI=
JWT_SECRET=
EMAIL_API_KEY=
```

Sensitive variables intentionally empty हैं। Comments developer को बताते हैं कि private value local
`.env` या approved secret manager से देनी है।

Unsafe template:

```dotenv
JWT_SECRET=actual-private-value
```

Filename में `example` होने से content automatically safe नहीं हो जाता।

## `.gitignore` verification

Current rules:

```gitignore
.env
.env.*
!.env.example
```

Verified decisions:

```text
taskforge-backend/.env         -> ignored
taskforge-backend/.env.local   -> ignored
taskforge-backend/.env.example -> visible/trackable
```

Command:

```powershell
git check-ignore -v --no-index -- taskforge-backend/.env
```

## Important limitation: ignore is prevention, eraser नहीं

```text
untracked secret + ignore rule -> normal add/status से बचाव
already tracked secret         -> ignore rule alone कोई effect नहीं
already committed secret       -> history/copies में रह सकता है
already pushed secret          -> remote clones/logs/caches में रह सकता है
```

`.gitignore` encryption, permission control या credential revocation नहीं करती।

## Configuration code se बाहर क्यों?

Same code different environments में चल सकता है:

```text
development -> dev database credential
test        -> test credential
production  -> production credential
```

Code repository में environment-specific private value hard-code करने से:

- credential developers/history तक फैलती है;
- rotate करना code change/deploy से tightly coupled होता है;
- accidental public exposure risk बढ़ता है;
- production और development isolation टूट सकती है।

Future Node phase में environment variables properly read/validate करेंगे। अभी केवल Git boundary है।

## Secrets कहाँ रखें?

Environment पर depend करता है:

```text
local development -> ignored .env + secure machine access
CI/CD              -> platform encrypted secret settings
production         -> hosting secret config / dedicated secret manager
```

Chat messages, screenshots, tickets, logs या command history भी secure secret stores नहीं हैं।

## Secret lifecycle

```text
generate strong secret
  -> distribute only to authorized systems/people
  -> store securely
  -> use with least privilege
  -> monitor/audit
  -> rotate periodically or on exposure
  -> revoke when no longer needed
```

Secret permanent magic string नहीं; lifecycle-managed credential है।

## Least privilege easy example

TaskForge email service credential को ideally केवल email send permission चाहिए, database delete या
cloud-admin permission नहीं। अगर leak हो भी जाए तो damage limited रहे:

```text
needed permission only + limited environment + expiration/rotation
```

## Prevention layers

One `.gitignore` enough नहीं। Layers:

```text
1. sensitive values code से बाहर
2. specific ignore rules
3. safe empty `.env.example`
4. exact staging instead of blind bulk add
5. status + cached diff review
6. automated secret scanning where available
7. protected branches/review/CI
8. short-lived, scoped credentials and rotation
```

No single scanner every secret detect कर सकता है।

## Manual pre-commit review

```powershell
git status --short
git diff
git diff --cached --name-status
git diff --cached
```

Look for:

```text
unexpected .env/key/certificate/credential file
authorization header value
database URI with username/password
private key block
token/API-key shaped value
debug logs containing credentials
real user/private data
```

Value दिखे तो उसे terminal/output/chat में copy मत करो। Path और incident category report करना often
enough है।

## Current safe audit evidence

Topic start पर tracked snapshot audit:

```text
tracked files checked: 71
sensitive-filename matches: 0
strong known credential-signature file matches: 0
```

Audit ने match values print नहीं कीं। Important limitation:

> Zero matches का मतलब “definitely no secrets” नहीं। Pattern scan limited है; unknown/custom secrets,
> encoded values, low-entropy passwords और historical/external copies miss हो सकती हैं।

Manual/context review और specialist scanning tools future team workflow में additional layer होंगे।

## If secret sirf working tree mein hai

Secret अभी commit नहीं हुई:

```text
1. Commit/push stop करो
2. File/value exposure scope assess करो
3. Staging में है तो working content delete किए बिना staged selection safely हटाओ
4. Secret को ignored/private storage में move करो
5. `.gitignore` rule verify करो
6. status + cached diff दुबारा review करो
7. अगर secret कहीं share/log हो चुकी है, rotate करो
```

## If secret commit hui but push नहीं हुई

```text
1. Push stop करो
2. Credential exposure assume/assess करो
3. Safest action: rotate/revoke credential
4. Current files/index clean करो और ignore policy fix करो
5. Local history correction strategy carefully choose करो
6. Verify history और staged diff
```

Even local commit IDE backup, logs या screen share में गई हो सकती है। “Remote पर नहीं गई” risk zero
नहीं बनाता। History rewrite command blindly नहीं चलाना।

## If secret remote par push ho gayi

Correct priority:

```text
1. STOP further sharing/use
2. REVOKE/ROTATE credential immediately
3. Notify repository/security/team owner
4. Audit logs and unauthorized use
5. Remove secret from current code/config
6. Decide coordinated history cleanup
7. Check forks, clones, CI logs, artifacts and caches
8. Add prevention and verify new credential
9. Document incident without writing secret value
```

सबसे important:

> **पहले credential invalid करो; सिर्फ Git history से text हटाना sufficient नहीं।**

## History cleanup caution

Tools/history rewrite से secret-containing commits replace हो सकते हैं, but:

- commit hashes बदलेंगे;
- force update required हो सकती है;
- teammates के clones diverge होंगे;
- forks/caches/backups फिर भी copies रख सकते हैं;
- published tags/releases/CI artifacts separately affected हो सकते हैं।

Therefore repository owner/security team के साथ coordinated plan चाहिए। This lesson कोई destructive
history rewrite execute नहीं करता।

## Rotation ka easy meaning

```text
old leaked key -> revoke/disable
new key        -> generate securely
authorized env -> new key install
application    -> verify
logs/audit     -> old key misuse check
```

Old secret को सिर्फ rename करना rotation नहीं।

## False positive kya hai?

Scanner कभी documentation placeholder या random string को secret समझ सकता है। Safe response:

```text
alert inspect करो
real credential 여부/context verify करो
value expose किए बिना finding classify करो
real हो तो incident process
fake हो तो documented suppression/narrow fixture
```

Scanner bypass/disable default fix नहीं।

## False negative kya hai?

Scanner “clean” बोले but secret exist करे। Reasons:

- custom format;
- short/simple password;
- encoded/encrypted-looking string;
- secret split across files;
- historical commit not scanned;
- external artifact/log not scanned।

इसलिए layers और human review चाहिए।

## Logging safety

Backend में future errors log करेंगे, लेकिन:

```text
Never log full Authorization header
Never log passwords
Never log reset tokens/session cookies
Never log complete database credential URI
Mask/redact sensitive fields
```

Debugging convenience secret exposure justify नहीं करती।

## Data/control flow

Safe runtime concept:

```text
secure environment/secret store
  -> process receives value at runtime
  -> code uses value in memory
  -> logs/responses redact it
  -> Git stores only code + safe variable template
```

Current phase में runtime code नहीं; only repository safety foundation है।

## Safe Git secret workflow

```text
status and filenames inspect
  -> work/cached diff context review
  -> ignore decisions verify
  -> safe template track
  -> scan staged + relevant history with appropriate tools
  -> commit only non-secret content
  -> push
  -> monitor and rotate on exposure
```

## Common errors aur easy fixes

### `.env` ignored है, इसलिए secret completely safe है

False. File backups, logs, terminal history, malware या previous commits से leak हो सकती है। Ignore
only one preventive layer है।

### Secret commit हुई, फिर line delete कर दी

Old commit में value remain कर सकती है। Rotate/revoke और history/external copies assess करो।

### GitHub repository private है, इसलिए secrets commit कर सकते हैं

Private repo access limited हो सकता है, but accounts, integrations, forks, logs या permissions
compromise हो सकती हैं। Secrets source history में रखने का default justification नहीं।

### Scanner zero findings = guaranteed safe

No. Scanner coverage limited है। Manual review, scoped credentials और other controls जरूरी हैं।

### `.env.example` में real value डाल दी

Example file trackable है। Values remove करो, credential rotate if exposed, template में blank/safe
placeholder रखो।

### Secret हटाने के लिए force push कर दिया

Team coordination और rotation के बिना incomplete/risky response है। Existing copies remain कर सकती
हैं और collaborators की history टूट सकती है।

### Error screenshot में token दिख गया

Git के बाहर भी exposure incident है। Credential rotate/revoke, shared artifact remove और audience/
logs audit करो।

## Student exercise

1. Secret क्या है?
2. `.env` और `.env.example` में क्या difference है?
3. `.gitignore` already committed secret हटा देती है?
4. Pushed secret मिलने पर first technical priority क्या है?
5. Scanner zero match क्या guarantee देता है?
6. JWT secret को logs में print करना चाहिए?

## Exercise answers

1. Private value जो protected access/action enable कर सकती है।
2. `.env` private runtime values; `.env.example` safe names/empty placeholders template.
3. नहीं।
4. Credential revoke/rotate करके उसे invalid करना।
5. कोई absolute guarantee नहीं; only checked patterns में finding नहीं मिली।
6. नहीं, redact/never log करना चाहिए।

## Interview question with Hinglish answer

**Question:** अगर secret GitHub पर push हो जाए तो आप क्या करेंगे?

**Answer:** मैं तुरंत further sharing रोककर credential revoke/rotate करूँगा, क्योंकि history से line
delete करना leaked credential को invalid नहीं करता। फिर team/security owner को inform, access logs
audit, current code से secret remove, ignore/secret-storage policy fix और coordinated history cleanup
करूँगा। Forks, CI logs, artifacts और clones भी assess करूँगा।

## Easy-English minimum interview answer

> If a secret is pushed, I first revoke or rotate it because deleting the line does not invalidate
> an exposed credential. Then I notify the responsible team, audit possible use, remove it from the
> current code, fix secret storage and ignore rules, and coordinate any history cleanup. I also
> check clones, forks, CI logs, and artifacts.

## Completion boundary

Topic 56 में safe `.env.example`, ignore behavior, limited non-value-revealing audit, prevention layers
और incident response documented/verified हुए। No real secret, `.env`, destructive history rewrite,
credential operation, commit या push हुआ। Topic 57 — Initial repository commit अभी start नहीं हुआ।
