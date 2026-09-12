# Topic 13 — Primary use cases

## Aaj ka exact objective

Aaj TaskForge ko user goals ke perspective se define karna hai. Hum primary use cases
identify aur structure karenge. System components ka high-level diagram Topic 14
mein aur APIs/code later phases mein aayenge.

## Use case ki simple definition

**Use case description hoti hai ki koi actor system ke saath interact karke ek useful
goal kaise achieve karta hai, including success aur important failure outcomes.**

Simple formula:

```text
Actor + goal + system interaction + observable outcome = use case
```

## Actor kya hai?

Actor woh role/person/external participant hai jo goal initiate karta hai. Actor ka
matlab database model ya specific code class hona zaroori nahi.

TaskForge actors:

- registered user;
- workspace owner;
- workspace admin;
- workspace member;
- system administrator;
- later, scheduled/external system actor bhi possible.

## Feature aur use case ka difference

- **Feature:** system capability—“task management”.
- **Use case:** actor goal—“workspace member valid task create karta hai”.

Feature noun/capability ho sakti hai. Use case action, context aur outcome batata hai.

## Use-case structure

### Actor

Kaun operation initiate karta hai?

### Goal

Actor kya useful outcome chahta hai?

### Trigger

Use case kis event/action se start hota hai?

### Preconditions

Start se pehle kya true hona chahiye?

### Main success flow

Correct conditions mein system kya business steps perform karta hai?

### Alternative/failure flows

Input invalid, permission missing ya dependency unavailable ho to kya hoga?

### Postcondition

Success ya failure ke baad system state kya honi chahiye?

## Complete example — Create task

**Actor:** authorized workspace member.

**Goal:** project ke andar actionable task record karna.

**Trigger:** member create-task operation initiate karta hai.

**Preconditions:**

- user identity established ho;
- user relevant workspace/project access rakhta ho;
- project usable state mein ho.

**Main flow:**

```text
1. Actor task information provide karta hai.
2. System input validate karta hai.
3. Workspace/project access verify hota hai.
4. Business rules check hote hain.
5. Task state persist hoti hai.
6. Safe created-task outcome actor ko milta hai.
```

**Failure flows:**

- title missing -> validation failure, task create nahi;
- no membership/access -> authorization failure, task create nahi;
- project archived/missing -> controlled business/not-found failure;
- database failure -> success falsely report nahi.

**Success postcondition:** exactly one valid task intended project/workspace mein
exists.

**Failure postcondition:** unauthorized/invalid attempt forbidden task create nahi
karti.

## Primary use-case groups

### Identity and account

1. User registers an account.
2. User logs in/out and manages session.
3. User manages own profile.
4. User verifies email or resets password when introduced.

### Workspace and membership

5. Registered user creates workspace and becomes its owner.
6. Owner/admin invites a member.
7. Invited user accepts valid invitation.
8. Authorized manager changes member role or removes member.
9. Owner archives/restores workspace within lifecycle rules.

### Project management

10. Authorized owner/admin creates project.
11. Authorized participant views/updates project.
12. Authorized manager manages membership/status/archive lifecycle.

### Task and issue management

13. Authorized member creates task/issue.
14. Member reads allowed task details/lists.
15. Authorized actor updates task fields.
16. Authorized actor assigns task to valid member.
17. Authorized actor changes task status through valid transition.
18. Actor manages labels, subtasks/dependencies when supported.

### Collaboration and traceability

19. Authorized member adds/edits/archives own allowed comment.
20. Authorized member attaches/accesses allowed file.
21. Member views relevant activity history.
22. Authorized authority views protected audit information.

### Search, insight and notification

23. Member filters/sorts/pages/searches accessible work.
24. Authorized user views workspace/project analytics.
25. User receives and manages relevant notifications.

### Platform operations

26. System administrator checks platform health/metrics.
27. System administrator performs audited moderation/operational action.

These are product requirements, implemented endpoints nahi.

## Primary versus supporting use case

Primary use case direct actor value deta hai, jaise “create task”. Supporting
behaviour primary flow enable karta hai, jaise:

- input validation;
- permission check;
- audit event record;
- notification schedule;
- log creation.

Supporting behaviour important hai, lekin actor ka main goal nahi ho sakta.

## Happy path enough kyun nahi?

Only success flow incomplete requirement hai. Backend ko define karna hoga:

- invalid input par state unchanged?
- unauthorized actor par no forbidden side effect?
- duplicate/conflicting action ka result?
- dependency failure par truthful outcome?
- sensitive data response/log mein excluded?

Failure paths later tests ke important cases banenge.

## Preconditions aur validation same nahi

Precondition requirement state describe karti hai—jaise user project member hona
chahiye. Validation/check code us requirement ko verify karega. Requirement aur
implementation separate rakhna useful hai.

## Postcondition kyun important hai?

Postcondition observable system state batati hai.

```text
Success: task persisted in correct project
Failure: invalid/unauthorized task not persisted
```

Response message alone enough nahi; side effect bhi expected state se match hona
chahiye.

## Use case se future implementation

```text
Use-case requirement
  -> API contract
  -> validation/auth/business rules
  -> data model and operations
  -> success/failure responses
  -> automated tests
```

Hum aaj future layers generate nahi kar rahe.

## Common misconceptions

1. **"Use case feature list hai."**  
   Use case actor, goal, flow and outcome add karta hai.

2. **"Use case mein code/function names hone chahiye."**  
   Product-level use case implementation-independent ho sakta hai.

3. **"Happy path document karna enough hai."**  
   Important invalid/unauthorized/failure outcomes required hain.

4. **"Response success hai to operation definitely correct hai."**  
   Expected side effect/postcondition verify karni hoti hai.

5. **"Logged-in user har use case perform kar sakta hai."**  
   Workspace/resource-scoped authorization still required hai.

6. **"Supporting log/notification primary goal hai."**  
   Usually woh primary business use case ko support karta hai.

## Verification strategy

Topic understood hai agar learner:

1. use case define kar sake;
2. actor, goal, trigger, precondition, flow and postcondition identify kare;
3. feature/use-case difference samjhaye;
4. create-task success and two failure flows explain kare;
5. failure par forbidden side effect absent hone ki importance bataye;
6. primary/supporting behaviour distinguish kare.

## Quick self-check

1. “Task management” feature hai ya complete use case?
2. Create-task actor aur goal kya hain?
3. Precondition aur postcondition mein difference?
4. Unauthorized failure ke baad expected state kya hai?
5. Audit log primary goal hai ya supporting behaviour?
6. Use case se future test kaise derive hoga?

## Practice exercise

**Invite workspace member** use case fill karo:

```text
Actor:
Goal:
Trigger:
Preconditions:
Main success flow:
Two failure flows:
Success postcondition:
Failure postcondition:
One supporting behaviour:
```

<details>
<summary>Answer-after-attempt</summary>

```text
Actor: Workspace owner or permitted admin
Goal: Valid person ko workspace join karne ka controlled invitation dena
Trigger: Actor invite-member operation initiates
Preconditions: Authenticated actor, correct workspace, invite permission
Success flow: Input validate -> permission check -> invitation create -> safe outcome
Failures: Invalid email; actor lacks permission
Success postcondition: One valid pending invitation exists
Failure postcondition: Unauthorized/invalid invitation does not exist
Supporting behaviour: Audited activity or delivery scheduling
```

</details>

## Interview question with Hinglish answer

**Question:** Use case kya hota hai? TaskForge example do.

**Answer:** Use case describe karta hai ki actor system ke saath interact karke useful
goal kaise achieve karta hai. Ismein actor, trigger, preconditions, main success flow,
failure flows aur postconditions hote hain. TaskForge create-task use case mein
authorized member input deta hai, system validation/access/rules check karta hai,
valid task persist karta hai aur safe result deta hai. Failure par unauthorized ya
invalid task create nahi hona chahiye.

## Easy-English minimum interview answer

**A use case describes how an actor interacts with a system to achieve a goal. It
includes the actor, preconditions, main flow, failure flows, and expected result. For
example, in TaskForge an authorized member creates a task, and invalid or unauthorized
attempts must not create any task.**

### Even shorter version

**A use case describes an actor's goal, the system interaction, and the expected
success or failure outcome.**

## Topic boundary

Topic 13 mein primary use cases complete hue. **High-level system diagram** Topic 14
ko iske baad separately complete kiya gaya.
