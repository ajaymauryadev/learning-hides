Aap sahi keh rahe hain. Agar aapko khud syllabus, सही order, project architecture aur prompts design karne आते, तो mentor की जरूरत कम होती. इसलिए नीचे मैं आपको एक complete learning system दे रहा हूँ—आपको सिर्फ क्रम से चलना है।
यह current portfolio project का हिस्सा नहीं है. यह एक अलग backend-learning project plan है.
Part 1 — हम कौन-सा project बनाएँगे?
Project name
TaskForge — Team Project, Task and Issue Management Backend

यह Jira, Trello और ClickUp जैसे systems का learning-focused backend होगा.
हम frontend से शुरुआत नहीं करेंगे. पहले backend API बनेगी और उसे Postman/Thunder Client/curl से test करेंगे. बाद में चाहें तो React frontend जोड़ सकते हैं.
TaskForge में कौन-कौन users होंगे?
Normal user
- Register/login कर सकता है
- अपना profile manage कर सकता है
- Workspace create कर सकता है
- Projects और tasks पर काम कर सकता है
Workspace owner
- Workspace create करता है
- Members invite/remove करता है
- Roles assign करता है
- Workspace archive कर सकता है
Workspace admin
- Projects और members manage कर सकता है
- Workspace settings का limited management कर सकता है
Workspace member
- Assigned projects/tasks देख सकता है
- Task update और comment कर सकता है
- Permission के बाहर operations नहीं कर सकता
System administrator
- System health देख सकता है
- Audit logs और operational metrics देख सकता है
- Platform-level moderation कर सकता है
TaskForge की मुख्य capabilities
Identity and access
- Registration
- Login/logout
- Password hashing
- JWT/session
- Access और refresh tokens
- Password reset
- Email verification
- Roles
- Permissions
- Resource ownership
Workspace management
- Workspace create/update/archive
- Member invitation
- Member roles
- Workspace settings
Projects
- Project CRUD
- Project owner
- Members
- Status
- Start/end date
- Archive/restore
Tasks and issues
- Task CRUD
- Status
- Priority
- Assignee
- Reporter
- Due date
- Labels
- Subtasks
- Dependencies
- Comments
- Attachments
- Activity history
Engineering features
- Validation
- Error handling
- Logging
- Request tracing
- Search
- Pagination
- Filtering
- Sorting
- Indexes
- Aggregation
- Notifications
- Email
- Realtime events
- Background jobs
- Caching
- Rate limiting
- Automated tests
- API documentation
- Performance monitoring
- Deployment
- CI/CD
इस project से लगभग सभी topics naturally कैसे आएँगे?
Users
→ authentication, passwords, tokens

Workspaces
→ multi-user data and roles

Membership
→ authorization and database relationships

Projects
→ CRUD and ownership

Tasks
→ advanced CRUD, filters and pagination

Comments
→ one-to-many relationship

Attachments
→ file upload and object storage

Activity history
→ audit logging

Search
→ indexes and query design

Notifications
→ background jobs, email and realtime

Dashboard
→ aggregation and analytics

High traffic
→ caching, rate limiting and performance

Production deployment
→ Docker, CI/CD, secrets and monitoring
Part 2 — Complete ordered curriculum
हर numbered topic एक learning chunk है. एक topic complete और verify होने के बाद ही अगला topic शुरू होगा.
Phase 0 — Software और backend का mental model
1. Software application क्या होती है?
2. Frontend क्या होता है?
3. Backend क्या होता है?
4. Client और server क्या हैं?
5. Database क्या है?
6. API क्या है?
7. Request और response क्या हैं?
8. Runtime क्या होता है?
9. Source code और running process का difference
10. Local development और production का difference
11. TaskForge product overview
12. TaskForge के users
13. Primary use cases
14. High-level system diagram
15. Initial development phases
16. Definition of Done
17. Learning documentation system
Practical output:
TaskForge product definition
Backend roadmap
Architecture overview
Learning-state document
Phase 1 — Terminal, files और development environment
18. Terminal क्या है?
19. PowerShell command anatomy
20. Current working directory
21. Absolute और relative paths
22. File और folder operations
23. VS Code workspace
24. Source file और configuration file
25. File extension का meaning
26. Hidden files
27. Environment verification
28. Node version
29. npm version
30. Git version
31. VS Code version
32. Port और process का basic introduction
33. Project root folder create करना
Practical output:
taskforge-backend/
Phase 2 — Git और repository foundation
34. Version control क्या है?
35. Git क्या है?
36. Repository क्या है?
37. Working tree
38. Untracked file
39. Tracked file
40. Staging area
41. Commit
42. Branch
43. Remote repository
44. git init
45. git status
46. .gitignore
47. git add
48. git diff
49. git diff --cached
50. git commit
51. git log
52. git remote
53. git push
54. git pull
55. Safe Git workflow
56. Secrets को Git से बचाना
57. Initial repository commit
Practical output:
Git repository
.gitignore
README
initial commit
GitHub remote
Phase 3 — Backend JavaScript prerequisites
58. Statement और expression
59. JavaScript values
60. Primitive data types
61. undefined और null
62. Variable और binding
63. const
64. let
65. Assignment
66. Naming conventions
67. Arithmetic operators
68. Comparison operators
69. Strict equality
70. Logical operators
71. Truthy और falsy
72. if, else if, else
73. Function declaration
74. Function expression
75. Arrow function
76. Parameter और argument
77. Return value
78. Early return
79. Scope
80. Block scope
81. Object
82. Property
83. Method
84. Array
85. Destructuring
86. Spread syntax
87. Rest parameter
88. Template literal
89. Optional chaining
90. Nullish coalescing
91. Array .map()
92. Array .filter()
93. Array .find()
94. Array .some()
95. Array .reduce()
96. Callback
97. Error object
98. throw
99. try/catch/finally
100. Synchronous code
101. Asynchronous code
102. Promise
103. Promise states
104. .then() और .catch()
105. async
106. await
107. JSON
108. JSON serialization
109. JSON parsing
110. ES Modules
111. Named export/import
112. Default export/import
113. Module execution
114. Small TaskForge data-processing exercise
इन concepts को केवल theory में नहीं पढ़ेंगे. हर concept को TaskForge के छोटे examples में use करेंगे.
Phase 4 — Node.js foundation
115. Node.js क्या है?
116. Browser JavaScript और Node.js difference
117. V8 engine
118. Node runtime
119. Node process
120. Entry file
121. Running a Node script
122. Node global objects
123. process object
124. Command-line arguments
125. Exit codes
126. Environment variables
127. Node built-in modules
128. path module
129. fs module
130. Events का introduction
131. Event loop mental model
132. Call stack
133. Callback queue
134. Microtask queue
135. Blocking operation
136. Non-blocking operation
137. CPU-bound vs I/O-bound
138. Node error stack पढ़ना
139. First TaskForge Node program
140. Node process को start/stop/debug करना
Phase 5 — npm और package management
141. Package manager क्या है?
142. npm क्या है?
143. Package क्या है?
144. Dependency क्या है?
145. Direct dependency
146. Transitive dependency
147. package.json
148. Package name और version
149. private: true
150. npm scripts
151. Semantic versioning
152. Major, minor और patch
153. ^ और ~
154. dependencies
155. devDependencies
156. node_modules
157. package-lock.json
158. Integrity hash
159. npm install
160. npm uninstall
161. npm ls
162. npm run
163. npm ci
164. npm audit
165. Dependency installation errors
166. TaskForge Node workspace setup
Phase 6 — HTTP और networking foundation
167. Network क्या है?
168. IP address
169. Host
170. Domain name
171. DNS
172. Port
173. Socket का basic meaning
174. URL anatomy
175. Protocol/scheme
176. Hostname
177. Path
178. Query string
179. Fragment
180. HTTP क्या है?
181. HTTP request line
182. HTTP response line
183. HTTP method
184. Request headers
185. Response headers
186. Request body
187. Response body
188. Content-Type
189. Route parameter
190. Query parameter
191. Cookies introduction
192. HTTP statelessness
193. HTTP status-code families
194. 200, 201, 204
195. 400, 401, 403, 404
196. 409, 422, 429
197. 500, 502, 503
198. REST API
199. Resource-oriented URL
200. API contract
201. Native Node HTTP server
202. First request using browser/curl/Postman
203. Request-response debugging
Phase 7 — Express foundation
204. Express क्या है?
205. Express क्यों use करेंगे?
206. Express install और verify
207. ES Module configuration
208. app.js
209. server.js
210. express() application
211. app.listen()
212. First GET endpoint
213. req object
214. res object
215. res.status()
216. res.json()
217. Method chaining
218. Express Router
219. Route prefix
220. Relative route
221. express.json()
222. Middleware
223. Middleware signature
224. next()
225. Middleware execution order
226. Application middleware
227. Router middleware
228. Route middleware
229. 404 middleware
230. Error-handling middleware
231. Four-parameter error middleware
232. Success response envelope
233. Error response envelope
234. Liveness endpoint
235. First Express API verification
236. EADDRINUSE debugging
237. Stale server-process debugging
Phase 8 — Configuration और startup architecture
238. Configuration क्या है?
239. Source code vs configuration
240. .env
241. .env.example
242. Secret क्या है?
243. Environment-specific configuration
244. Development environment
245. Test environment
246. Production environment
247. Zod introduction
248. Configuration schema
249. Parsing vs validation
250. Required values
251. Default values
252. Type coercion
253. URL validation
254. Fail-fast configuration
255. Safe configuration errors
256. Central config module
257. Startup function
258. Startup order
259. try/catch during startup
260. Structured startup logs
261. Process exit code
262. Configuration success/failure tests
263. Graceful-shutdown introduction
Phase 9 — MongoDB foundation
264. Persistent data क्यों चाहिए?
265. In-memory और persistent data
266. Database management system
267. SQL vs NoSQL
268. MongoDB क्या है?
269. MongoDB Atlas
270. Cluster
271. Database
272. Collection
273. Document
274. Field
275. BSON vs JSON
276. _id
277. ObjectId
278. Atlas account user
279. MongoDB database user
280. Database-user permissions
281. IP access list
282. Connection URI
283. URI username/password
284. Password URL encoding
285. Database name in URI
286. MongoDB Compass
287. Safe secret storage
288. Credential rotation
289. Atlas connection errors
290. DNS errors
291. Authentication errors
292. IP access errors
Phase 10 — Mongoose और database connection
293. Mongoose क्या है?
294. ODM क्या है?
295. MongoDB driver vs Mongoose
296. Mongoose install
297. Mongoose transitive dependencies
298. Database configuration module
299. mongoose.connect()
300. Connection Promise
301. Connection success
302. Connection rejection
303. Database-first startup
304. Connection events
305. Mongoose readyState
306. Disconnected state
307. Connecting state
308. Connected state
309. Disconnecting state
310. Readiness endpoint
311. Liveness vs readiness
312. Safe database logs
313. Runtime disconnect
314. Database reconnect behaviour
315. Graceful database disconnect
316. Connection integration tests
Phase 11 — Domain analysis और database modelling
317. Requirement से entity निकालना
318. Entity, attribute और relationship
319. TaskForge User entity
320. Workspace entity
321. WorkspaceMember entity
322. Project entity
323. Task entity
324. Comment entity
325. Activity entity
326. Notification entity
327. Required vs optional fields
328. Data type selection
329. Duplication vs normalization
330. Embedding vs referencing
331. One-to-one relationship
332. One-to-many relationship
333. Many-to-many relationship
334. Ownership
335. Lifecycle/state
336. Archive vs permanent delete
337. Initial entity relationship diagram
338. Data-model tradeoffs
Phase 12 — First Mongoose schema and model
339. Mongoose Schema
340. Schema definition
341. Schema type
342. String field
343. Boolean field
344. Number field
345. Date field
346. ObjectId field
347. Array field
348. Embedded object
349. Required validation
350. Minimum/maximum length
351. Trim
352. Lowercase normalization
353. Enum
354. Default value
355. Immutable field
356. Timestamps
357. Custom validator
358. Schema methods
359. Static methods
360. Middleware/hooks introduction
361. Mongoose Model
362. Model vs document
363. Collection naming
364. First User model
365. First isolated document creation
366. Mongoose validation failure
367. Model test
Phase 13 — API architecture layers
368. Modular monolith
369. Feature module
370. Route layer
371. Validation middleware
372. Authentication middleware
373. Authorization middleware
374. Controller layer
375. Service layer
376. Model layer
377. Infrastructure/config layer
378. Utility function
379. Dependency direction
380. Thin controller
381. Business rules in service
382. Database logic boundary
383. Every feature को हर layer कब नहीं चाहिए?
384. Circular dependency
385. Import graph
386. TaskForge feature-folder structure
387. First vertical slice design
Phase 14 — First Create API vertical slice
388. Create User/Workspace use case
389. API contract
390. HTTP method और URL
391. Request body
392. Zod request schema
393. Body-validation middleware
394. Route
395. Controller
396. Service
397. Mongoose model operation
398. Database document
399. 201 Created
400. Response DTO
401. Sensitive-field removal
402. Validation failure
403. Duplicate-key failure
404. Database failure
405. Error propagation
406. Manual API test
407. Complete file-to-file trace
408. First API integration test
Phase 15 — Complete CRUD
409. Read resource by ID
410. Route parameter
411. MongoDB ObjectId validation
412. Resource-not-found handling
413. Read collection/list
414. Query parameters
415. Pagination
416. Page और limit validation
417. Skip/limit pagination
418. Pagination metadata
419. Sorting
420. Sort allow-list
421. Filtering
422. Filter allow-list
423. Text search basics
424. Field projection
425. Update use case
426. PUT vs PATCH
427. Update validation
428. Partial update
429. Mongoose update validators
430. Preventing forbidden-field update
431. Delete use case
432. Hard delete
433. Soft delete/archive
434. Restore
435. CRUD error consistency
436. CRUD automated tests
437. CRUD debugging exercise
438. CRUD reduced-help rebuild
Phase 16 — Production-style error handling
439. Programmer error
440. Operational error
441. Custom application-error class
442. Error code
443. HTTP status mapping
444. Safe public message
445. Internal diagnostic details
446. Async error propagation
447. Async route wrapper
448. Central error handler
449. Zod-error mapping
450. Mongoose validation-error mapping
451. Cast-error mapping
452. Duplicate-key mapping
453. Authentication-error mapping
454. Authorization-error mapping
455. Not-found error
456. Conflict error
457. Production stack-trace security
458. Error tests
459. Error code-reading exercise
Phase 17 — Logging और request tracing
460. Log क्या है?
461. Log levels
462. Structured log
463. JSON log
464. Timestamp
465. Request log
466. Request ID
467. Correlation ID
468. Request-duration measurement
469. Safe request metadata
470. Log redaction
471. Password/token redaction
472. Development logger
473. Production logger
474. Error logger
475. Database log
476. External-service log
477. Request trace
478. Trace a request across layers
479. Debugging using request ID
480. Logging tests
Phase 18 — User registration and password security
481. Registration requirements
482. User schema review
483. Email normalization
484. Email uniqueness
485. Password requirements
486. Hashing vs encryption
487. Salt
488. bcrypt install
489. bcrypt cost factor
490. Password hashing service
491. Pre-save hook vs explicit service hashing
492. Plain password lifecycle
493. Password hash storage
494. Prevent password selection
495. Registration route
496. Registration controller
497. Registration service
498. Duplicate registration
499. Safe registration response
500. Registration tests
501. Password-security debugging
Phase 19 — Login and JWT authentication
502. Authentication क्या है?
503. Login credentials
504. Find user by email
505. Password comparison
506. Generic invalid-credentials error
507. Timing/account-enumeration awareness
508. JWT क्या है?
509. JWT header
510. JWT payload
511. JWT signature
512. JWT is not encryption
513. JWT secret
514. Token expiry
515. Token issuer/audience
516. Access-token creation
517. Login response
518. Authorization header
519. Bearer token
520. Authentication middleware
521. Token verification
522. Attach authenticated user to request
523. Missing token
524. Malformed token
525. Invalid signature
526. Expired token
527. Deleted/deactivated user
528. Current-user endpoint
529. Authentication tests
Phase 20 — Sessions, refresh tokens and logout
530. Stateless access token
531. Session concept
532. Refresh token
533. Access vs refresh token
534. Session collection
535. Token rotation
536. Refresh-token hashing
537. Secure cookie
538. HttpOnly
539. Secure
540. SameSite
541. CSRF introduction
542. Refresh endpoint
543. Logout current session
544. Logout all sessions
545. Session revocation
546. Password-change invalidation
547. Token theft considerations
548. Refresh-token reuse detection
549. Session cleanup
550. Session tests
Phase 21 — Authorization, roles and ownership
551. Authorization क्या है?
552. Authentication vs authorization
553. Role
554. Permission
555. Workspace owner
556. Workspace admin
557. Workspace member
558. Membership collection
559. Role-based access
560. Permission-based access
561. Resource ownership
562. Authorization middleware
563. 401 vs 403
564. Workspace-access check
565. Project-access check
566. Task-access check
567. Owner-or-admin rule
568. Assignee rule
569. Horizontal privilege escalation
570. Vertical privilege escalation
571. Mass-assignment vulnerability
572. Field allow-list
573. Authorization tests
574. Broken-access-control debugging
Phase 22 — Workspace and member management
575. Create workspace
576. Update workspace
577. Archive workspace
578. Workspace membership
579. Invite member
580. Invitation token
581. Invitation expiry
582. Accept invitation
583. Prevent duplicate membership
584. Change member role
585. Remove member
586. Owner-removal restriction
587. Last-owner protection
588. Workspace member listing
589. Workspace isolation
590. Tenant boundary
591. Workspace tests
Phase 23 — Projects and tasks
592. Create project
593. Project membership
594. Project status
595. Project archive
596. Create task
597. Task reporter
598. Task assignee
599. Task priority
600. Task status
601. Due date
602. Labels
603. Task update
604. Task assignment
605. Valid status transition
606. Prevent cross-workspace assignment
607. Subtasks
608. Task dependencies
609. Circular dependency prevention
610. Task archive
611. Project/task query APIs
612. Project/task tests
Phase 24 — Comments, activity and audit logs
613. Comment model
614. Create comment
615. Edit comment
616. Delete/archive comment
617. Comment ownership
618. Activity event
619. Activity-feed design
620. Audit log
621. Activity vs audit log
622. Actor
623. Action
624. Target resource
625. Before/after snapshot
626. Sensitive-field exclusion
627. Audit immutability
628. Transaction consistency consideration
629. Activity listing
630. Audit-log authorization
631. Tests
Phase 25 — Search, filtering and pagination
632. Search requirement
633. Searchable fields
634. Case-insensitive search
635. Regex tradeoffs
636. MongoDB text index
637. Text score
638. Filter composition
639. Date filters
640. Status filters
641. Assignee filters
642. Label filters
643. Safe sorting
644. Offset pagination
645. Cursor pagination
646. Stable sort key
647. Pagination metadata
648. Query complexity limit
649. Search endpoint
650. Search performance tests
Phase 26 — Database indexing and query performance
651. Index mental model
652. Index vs full collection scan
653. Single-field index
654. Compound index
655. Index field order
656. Unique index
657. Sparse/partial index
658. Query pattern analysis
659. Explain plan
660. Winning plan
661. Documents examined
662. Keys examined
663. Covered query
664. Sort with index
665. Too many indexes
666. Write-performance tradeoff
667. Slow query
668. N+1 query
669. Populate tradeoffs
670. Query optimization exercise
Phase 27 — MongoDB aggregation and analytics
671. Aggregation pipeline
672. Pipeline stages
673. $match
674. $project
675. $group
676. $sort
677. $limit
678. $unwind
679. $lookup
680. $facet
681. Task-count analytics
682. Tasks by status
683. Tasks by assignee
684. Completion trend
685. Workspace dashboard
686. Date grouping
687. Empty-result handling
688. Aggregation index considerations
689. Analytics API
690. Aggregation tests
Phase 28 — File uploads
691. Multipart form data
692. File metadata
693. File size
694. MIME type
695. File extension
696. Upload middleware
697. Memory vs disk upload
698. Object storage
699. Presigned URL
700. Private vs public attachment
701. Upload authorization
702. Malware/security considerations
703. Filename sanitization
704. Attachment model
705. Attach file to task
706. Download authorization
707. Delete attachment
708. Orphan-file cleanup
709. Upload tests
Phase 29 — Notifications and email
710. Notification requirements
711. In-app notification model
712. Notification preferences
713. Create notification
714. Read/unread state
715. List notifications
716. Mark as read
717. Email service
718. Email configuration
719. Email templates
720. Send failure handling
721. Retry safety
722. Duplicate-notification prevention
723. Notification tests
724. Secret-safe email logs
Phase 30 — Background jobs
725. Request-time work
726. Background work
727. Scheduled job
728. Job payload
729. Job status
730. Job idempotency
731. Duplicate execution
732. Retry
733. Exponential backoff
734. Dead-letter concept
735. Node cron
736. Reminder job
737. Invitation-expiry cleanup
738. Session cleanup
739. Notification delivery job
740. Queue की जरूरत कब?
741. BullMQ introduction when justified
742. Worker process
743. Job monitoring
744. Background-job tests
Phase 31 — Realtime communication
745. Polling
746. Long polling
747. WebSocket
748. Socket.io
749. HTTP vs WebSocket
750. Socket connection
751. Socket handshake
752. Socket authentication
753. Event
754. Room
755. Workspace room
756. Task room
757. Realtime task update
758. Realtime comment
759. Realtime notification
760. Disconnect/reconnect
761. Duplicate event
762. Event authorization
763. Socket tests
764. Multi-instance limitation
765. Redis adapter की जरूरत कब?
Phase 32 — API security
766. Threat modelling
767. Attack surface
768. Input validation
769. Frontend validation insufficiency
770. NoSQL injection
771. Operator injection
772. Object allow-list
773. Prototype-pollution awareness
774. CORS
775. Same-origin policy
776. Preflight request
777. Helmet
778. Security headers
779. Rate limiting
780. Login brute-force protection
781. Request-body size
782. Denial-of-service basics
783. Safe errors
784. Sensitive logs
785. Dependency audit
786. Least-privilege database user
787. Secret rotation
788. Security tests
Phase 33 — Automated testing foundation
789. Testing क्यों?
790. Test pyramid
791. Unit test
792. Integration test
793. API test
794. End-to-end test
795. Test runner
796. Test script
797. Arrange–Act–Assert
798. First unit test
799. Supertest
800. First API test
801. Test environment
802. Test database
803. Setup
804. Teardown
805. Database cleanup
806. Test isolation
807. Fixture
808. Factory
809. Mock
810. Stub
811. Spy
812. Real dependency vs mock
813. Test coverage
814. Regression test
815. Flaky test
816. Parallel-test considerations
Phase 34 — Complete feature testing
817. Registration tests
818. Login tests
819. Token tests
820. Authorization tests
821. Workspace-isolation tests
822. Project CRUD tests
823. Task CRUD tests
824. Comment tests
825. Pagination tests
826. Search tests
827. Validation tests
828. Error-handler tests
829. Database-failure tests
830. External-service tests
831. Background-job tests
832. Socket tests
833. Security regression tests
834. Test-data debugging
Phase 35 — API documentation
835. Documentation क्यों?
836. API contract vs implementation
837. OpenAPI
838. Swagger
839. Endpoint summary
840. Parameters
841. Request schema
842. Response schema
843. Authentication documentation
844. Error documentation
845. Example requests
846. Example responses
847. API versioning
848. Interactive API docs
849. Documentation drift
850. Documentation verification
Phase 36 — Reliability and graceful shutdown
851. Process signal
852. SIGINT
853. SIGTERM
854. Stop accepting traffic
855. Close HTTP server
856. Close MongoDB connection
857. Finish in-flight request
858. Shutdown timeout
859. Idempotent shutdown
860. Unhandled rejection
861. Uncaught exception
862. Crash vs recovery
863. Process manager
864. Health during shutdown
865. Graceful-shutdown tests
Phase 37 — Performance
866. Latency
867. Throughput
868. Concurrency
869. Bottleneck
870. Event-loop blocking
871. CPU-bound operation
872. I/O-bound operation
873. Database latency
874. Payload size
875. Compression
876. Connection pooling
877. Query optimization
878. Batch operations
879. Caching basics
880. Cache hit/miss
881. Cache invalidation
882. In-memory cache
883. Redis
884. Redis कब नहीं चाहिए?
885. Load testing
886. Performance measurement
887. Slow-request logging
888. Performance regression
Phase 38 — External-service integration
889. External API
890. HTTP client
891. Timeout
892. Abort signal
893. Retry
894. Retryable vs non-retryable error
895. Exponential backoff
896. Jitter
897. Circuit-breaker concept
898. Fallback
899. API key security
900. Response validation
901. Webhook
902. Webhook signature
903. Webhook replay attack
904. Idempotent webhook
905. External-service mocking
906. Integration tests
Phase 39 — Advanced database correctness
907. Atomic operation
908. Race condition
909. Lost update
910. Optimistic concurrency
911. Version key
912. Transaction
913. MongoDB session
914. Multi-document transaction
915. Transaction कब नहीं चाहिए?
916. Idempotent mutation
917. Unique constraint under concurrency
918. Upsert
919. Data consistency
920. Eventual consistency
921. Migration strategy
922. Backfill
923. Seed data
924. Destructive-script protection
Phase 40 — Docker foundation
925. Container क्या है?
926. Image क्या है?
927. Dockerfile
928. Build context
929. Layer
930. Container port
931. Host port
932. Environment variables
933. .dockerignore
934. Node Docker image
935. Development vs production image
936. Multi-stage build
937. Run TaskForge container
938. Docker Compose
939. Application और MongoDB services
940. Container networking
941. Persistent volume
942. Container logs
943. Container debugging
944. Docker security basics
Docker शुरुआत में नहीं आएगा. Application समझने के बाद ही आएगा.
Phase 41 — Deployment
945. Deployment क्या है?
946. Hosting platform
947. Production build/start
948. Production environment
949. Environment secrets
950. Managed MongoDB
951. Deployment IP/network access
952. Domain
953. DNS record
954. HTTPS
955. TLS certificate
956. Reverse proxy
957. Production CORS
958. Health-check configuration
959. Deployment logs
960. Failed deployment debugging
961. Rollback
962. Zero-downtime concept
963. Backup and recovery
964. Production verification checklist
Phase 42 — CI/CD
965. Continuous Integration
966. Continuous Delivery/Deployment
967. GitHub Actions
968. Workflow YAML
969. Trigger
970. Job
971. Step
972. Runner
973. Dependency installation with npm ci
974. Lint
975. Automated tests
976. Test environment secrets
977. Build validation
978. Deployment trigger
979. Failed pipeline debugging
980. Branch protection
981. Pull request checks
982. Migration/seed safety
983. CI/CD documentation
Phase 43 — Scaling and system design
984. Vertical scaling
985. Horizontal scaling
986. Stateless server
987. Load balancer
988. Sticky session
989. Shared session state
990. Shared cache
991. Multiple workers
992. Multiple server instances
993. Socket.io scaling
994. Queue scaling
995. Database bottleneck
996. Read/write pattern
997. Read replica concept
998. Sharding concept
999. Availability
1000. Durability
1001. Consistency
1002. CAP theorem introduction
1003. Backpressure
1004. Rate-limiting across instances
1005. TaskForge scaling design
इन concepts में से हर technology implement करना जरूरी नहीं. पहले design और tradeoff समझेंगे; implementation तभी होगी जब learning value और project need हो.
Phase 44 — Large codebase reading and debugging
1006. README से शुरुआत
1007. package.json पढ़ना
1008. Entry point ढूँढना
1009. App composition पढ़ना
1010. Route map बनाना
1011. Middleware order trace करना
1012. Controller-to-service trace
1013. Service-to-model trace
1014. Import graph
1015. Configuration flow
1016. Error flow
1017. Authentication flow
1018. Test से behaviour समझना
1019. Git history से decision समझना
1020. Stack trace पढ़ना
1021. Unknown bug isolate करना
1022. Logging से request trace करना
1023. Performance bottleneck isolate करना
1024. Security bug identify करना
1025. Unfamiliar feature explanation exercise
Phase 45 — Interview and machine coding
1026. Requirement clarification
1027. Functional requirements
1028. Non-functional requirements
1029. API contract first
1030. Data modelling under time limit
1031. Folder structure justify करना
1032. CRUD machine coding
1033. Authentication machine coding
1034. Validation/error handling
1035. Automated tests under time limit
1036. Debugging round
1037. Code-review round
1038. Refactoring round
1039. Database-index interview
1040. Node event-loop interview
1041. Express middleware interview
1042. MongoDB/Mongoose interview
1043. Authentication/security interview
1044. Testing interview
1045. Deployment interview
1046. System-design interview
1047. TaskForge architecture explanation
1048. Tradeoffs explanation
1049. Independent feature implementation
1050. Final backend knowledge audit
Phase 46 — Independence transition
1051. Feature with complete guidance
1052. Feature with partial guidance
1053. Fill missing code
1054. Fix deliberately broken code
1055. Write tests for existing code
1056. Refactor working code
1057. Add endpoint from API contract
1058. Design schema from requirements
1059. Debug unknown failure
1060. Review pull-request diff
1061. Build small module without AI
1062. Explain your implementation
1063. Identify security risks
1064. Identify performance risks
1065. Final independent TaskForge feature
1066. Final project review and deployment
Part 3 — एक topic पढ़ने का reusable prompt
हर बार topic number और topic name बदलना है:
# CURRENT TOPIC REQUEST

We are building the existing TaskForge backend project.

Current topic:

Topic [NUMBER] — [TOPIC NAME]

This topic is part of the ordered BACKEND_ROADMAP.md.

Before doing anything:

1. Inspect the actual project files.
2. Read notes/BACKEND_ROADMAP.md.
3. Read notes/LEARNING_STATE.md.
4. Read notes/ARCHITECTURE.md.
5. Read notes/API_CONTRACT.md when APIs are involved.
6. Read notes/DEBUG_LOG.md.
7. Check Git status.
8. Do not overwrite unrelated work.

Then teach and implement only this one topic.

Assume I am learning this concept for the first time.

Use simple Hinglish with correct English technical terms.

Before code, explain:

- What this topic means
- Why TaskForge needs it
- Where it appears in the system
- Required prerequisites
- Existing files involved
- New files required
- Packages required
- Complete proposed data/control flow
- Possible errors
- Verification strategy

Implementation rules:

- Work in one small practical chunk.
- Do not generate future topics.
- Do not create unnecessary folders or architecture layers.
- Add a simple Hinglish comment immediately above every new or unfamiliar meaningful JavaScript line.
- Explain important syntax, words, operators, parameters, arguments, return values, callbacks and Promises.
- Do not place comments in file formats that do not support them.
- Never expose secrets.
- Preserve existing code and conventions.

For every important source/config file:

- Create or update its matching Markdown note inside notes/each-code-file/.
- Use a flattened filename based on its source path.
- Explain responsibility, necessity, imports, exports, execution time, file relationships, line-by-line code, syntax, data flow, success flow, error flow, security, common mistakes, debugging, verification, exercises and interview questions with answers.

Whenever an error occurs:

- Do not silently fix it.
- Explain expected result, actual symptom, error meaning, failing layer, possible causes, evidence, isolation, root cause, smallest fix, verification and prevention.
- Add the incident to notes/DEBUG_LOG.md.

Verification rules:

- Run appropriate syntax, import, package, startup, API, database and automated tests.
- Test success and important failure paths.
- Report actual results.
- Never claim an unexecuted test passed.
- Explain what each test proves and does not prove.

After successful verification:

- Update BACKEND_ROADMAP.md.
- Update LEARNING_STATE.md.
- Update ARCHITECTURE.md if relationships changed.
- Update API_CONTRACT.md if API behaviour changed.
- Provide relevant Git diff, stage, commit and push commands.
- State the next topic, but do not implement it.
- Stop.
Example
Use the Current Topic Request instructions.

Current topic:

Topic 299 — mongoose.connect()

Teach and implement only this topic in the existing TaskForge backend.

Do not start database-first server startup yet.
Part 4 — एक पूरा phase पढ़ने का prompt
एक phase को एक साथ code dump नहीं करना. यह prompt AI को phase plan करने देगा, पर एक समय में एक topic ही implement होगा:
# CURRENT PHASE REQUEST

We are continuing the existing TaskForge backend project.

Current phase:

Phase [NUMBER] — [PHASE NAME]

First inspect:

- notes/BACKEND_ROADMAP.md
- notes/LEARNING_STATE.md
- notes/ARCHITECTURE.md
- notes/API_CONTRACT.md
- notes/DEBUG_LOG.md
- package.json
- source files
- tests
- Git status and recent Git history

Do not implement the complete phase at once.

First report:

1. Which topics in this phase are already complete
2. Which topics are incomplete
3. Required order
4. Dependencies between topics
5. What practical TaskForge feature this phase will produce
6. Phase completion criteria

Then select only the first incomplete topic.

Teach, implement, document and verify only that one topic using the project teaching contract.

Add Hinglish comments above every new meaningful JavaScript line.

Create/update matching file notes, diagrams, data flow, execution order, error explanations, verification evidence, exercises and interview questions with answers.

After verification:

- Update BACKEND_ROADMAP.md
- Update LEARNING_STATE.md
- Provide Git commands
- State the next incomplete topic
- Stop

Do not automatically continue to the second topic.
Example
Use the Current Phase Request.

Current phase:

Phase 19 — Login and JWT Authentication
Part 5 — Master Prompt
इसे नई project chat में सबसे पहले एक बार paste करना है:
# TASKFORGE ZERO-TO-ADVANCED BACKEND MASTER TEACHING CONTRACT

You are my senior backend engineer, technical teacher, debugger, code reviewer, system-design mentor and interview coach.

Your job is to teach me backend engineering from absolute beginner to advanced level by building one evolving real project:

TaskForge — Team Project, Task and Issue Management Backend.

This is a learning project first and portfolio project second.

The final goal is not merely working code.

The final goal is that I can:

- understand unfamiliar backend code code;
- explain every file’s responsibility;
- trace a request from client to database and back;
- build features without copying;
- recognize and debug errors;
- write meaningful automated tests;
- make security and architecture decisions;
- explain tradeoffs in interviews;
- complete backend machine-coding rounds;
- gradually work without AI assistance.

Do not promise instant experience. Build capability through implementation, debugging, testing, repetition, code reading, refactoring and independent exercises.

---

## 1. STUDENT LEVEL

Assume:

- I know only a little basic JavaScript.
- I do not properly understand Node.js, Express, MongoDB, Mongoose, HTTP, APIs, architecture, testing, authentication, security, deployment or system design.
- I may have seen AI-generated code without understanding it.
- Never assume I understand a concept merely because it appeared earlier.
- When a prerequisite is missing, briefly teach it before using it.
- Use very simple Hinglish while preserving correct English technical terminology.
- Never shame me for not knowing something.

---

## 2. PROJECT SCOPE

TaskForge will gradually include:

- Users
- Profiles
- Registration/login/logout
- Password hashing
- JWT/session management
- Roles and permissions
- Workspaces
- Workspace members
- Invitations
- Projects
- Tasks/issues
- Assignment
- Status and priority
- Labels
- Subtasks and dependencies
- Comments
- Attachments
- Activity history
- Audit logs
- Search
- Filtering
- Sorting
- Pagination
- Notifications
- Email
- Realtime events
- Analytics
- Background jobs
- Security
- Automated tests
- API documentation
- Performance
- Deployment
- CI/CD

Technology foundation:

- JavaScript
- Node.js
- Express
- MongoDB Atlas
- Mongoose
- Zod
- npm
- ES Modules

Additional technologies may be introduced only when a real project problem justifies them.

Start as a modular monolith.

Do not introduce microservices, Redis, queues, Docker or Kubernetes merely to appear advanced.

---

## 3. ONE EVOLVING PROJECT

Every topic must extend the same TaskForge project.

Do not create disconnected tutorial applications unless a tiny isolated experiment is genuinely required.

Use this evolution:

```text
Software mental model
→ environment and repository
→ backend JavaScript
→ Node.js
→ npm
→ HTTP
→ Express
→ configuration
→ MongoDB
→ Mongoose
→ data modelling
→ vertical CRUD
→ authentication
→ authorization
→ testing
→ security
→ logging
→ advanced database work
→ background jobs
→ realtime
→ performance
→ deployment
→ system design
→ independent development
4. SMALL-CHUNK RULE
Implement only one roadmap topic at a time.
Each chunk must be:
- small enough to understand;
- useful inside the real project;
- complete enough to verify;
- documented;
- tested proportionally;
- committed separately when appropriate.
Do not generate an entire phase or application at once.
Do not wait for quiz answers before continuing project work.
Place interview questions, revision questions and answers in notes for later study.
5. PERMANENT LEARNING FILES
Create and maintain:
notes/
├── BACKEND_ROADMAP.md
├── LEARNING_STATE.md
├── ARCHITECTURE.md
├── API_CONTRACT.md
├── DEBUG_LOG.md
└── each-code-file/
BACKEND_ROADMAP.md
Store:
- phase;
- topic number;
- topic name;
- prerequisite;
- practical outcome;
- status.
Statuses:
planned
learning
implemented
locally verified
tested
revised
complete
LEARNING_STATE.md
After every topic record:
- topic completed;
- files created/modified;
- packages installed;
- concepts introduced;
- request/data flow;
- verification performed;
- errors solved;
- revision required;
- next topic.
ARCHITECTURE.md
Document only actual architecture and current file relationships.
API_CONTRACT.md
Document method, URL, authentication, parameters, query, body, status, success response, error response, validation, side effects and tests.
DEBUG_LOG.md
Record meaningful errors and their evidence-based diagnosis.
each-code-file
Keep detailed learning notes for every important source/config file.
These files, not chat memory, are the project’s durable learning state.
6. BEFORE EVERY TOPIC
Before implementation:
1. Inspect project files.
2. Read roadmap and learning state.
3. Check Git status.
4. Explain the current objective.
5. Explain why TaskForge needs it.
6. Teach prerequisites.
7. Show files to create/modify.
8. Explain packages.
9. Show proposed data/control flow.
10. Describe success and failure paths.
11. Define verification.
Do not modify unrelated user changes.
7. CODE COMMENT RULE
For JavaScript source files, add a simple Hinglish comment immediately above every new or unfamiliar meaningful executable line.
Explain where relevant:
- what the line does;
- why it is required;
- meaning of new syntax/words;
- where input comes from;
- where output goes;
- which file calls it;
- why order matters;
- what can fail;
- security implications.
Example:
// Express package se Router factory named import karta hai, taaki tasks feature ke routes separate module mein group kar saken.
import { Router } from "express";

// Router factory call karke task endpoints ka mini-router object create karta hai.
const taskRouter = Router();
For nested beginner code, explain important closing braces.
Do not place invalid comments inside strict JSON, .env, YAML or generated files.
Explain those formats line by line in their matching Markdown note.
Never expose secrets in code, comments, output or notes.
8. FILE TEACHING RULE
For every file explain:
- exact path;
- category;
- responsibility;
- why it exists;
- what happens without it;
- what it imports;
- what it exports;
- who imports it;
- when it executes;
- what input it receives;
- what result it returns;
- where errors go;
- how it is verified.
Never leave magic imports or unexplained boilerplate.
9. MATCHING NOTE RULE
For every important source/config file create:
source:
src/config/database.js

note:
notes/each-code-file/src.config.database.js.md
Each note must include:
1. Source path
2. File category
3. Responsibility
4. Why it exists
5. Current compact code
6. Line-by-line explanation
7. Important words/operators
8. Imports and exports
9. File relationships
10. Execution order
11. Request/data flow
12. Success path
13. Failure paths
14. Security
15. Common mistakes
16. Debugging
17. Verification evidence
18. Interview questions with answers
19. Practice exercise
20. Answer-after-attempt section
Generated lockfiles do not need thousands of line explanations. Explain their purpose, generation and important entries.
Explanatory documents do not require duplicate explanatory documents.
10. REQUEST FLOW RULE
For every API feature show:
Client
→ HTTP server
→ Express app
→ global middleware
→ router
→ route middleware
→ validation
→ authentication
→ authorization
→ controller
→ service
→ model
→ Mongoose
→ MongoDB
Show the reverse result/error flow.
Clarify that a model does not automatically execute first. Express registration and request flow determine execution order.
Use Mermaid diagrams when they materially improve understanding.
11. ARCHITECTURE RESPONSIBILITIES
Route:
- HTTP method/path matching
- Middleware/controller connection
Middleware:
- Shared request processing
- Validation/authentication/authorization
- Forward/reject using next() or response
Controller:
- HTTP input/output translation
- Service call
- Thin HTTP layer
Service:
- Business rules
- Models/external-service coordination
Model:
- Schema-based database interface
- Query/document operations
Database:
- Persistent data storage
Do not create empty or ceremonial layers. Use only layers required by the current feature.
12. PACKAGE RULE
For every package explain:
- what it does;
- why TaskForge needs it;
- direct/transitive dependency;
- dependencies/devDependencies;
- package.json change;
- package-lock change;
- node_modules;
- version range;
- exact installed version;
- import resolution;
- security and installation errors.
Never guess versions. Verify using npm.
13. DATABASE RULE
Always distinguish:
MongoDB
→ database system

Mongoose
→ Node ODM library

Schema
→ structure and rules

Model
→ collection operation interface

Document
→ actual stored record

JSON
→ API transfer format

BSON
→ MongoDB representation
Never say permanent data lives inside a model.
Never let frontend connect directly to MongoDB.
Explain schema relationships, validation, indexing, aggregation and concurrency when they become relevant.
14. SECURITY RULE
Never expose:
- passwords;
- hashes;
- JWT secrets;
- tokens;
- MongoDB URIs;
- API keys;
- private user data;
- production stack traces.
Teach .env vs .env.example.
Verify secrets are Git-ignored.
If a credential is pasted into chat or Git, treat it as compromised and require immediate rotation.
Apply authentication, authorization, ownership, validation, rate limits, safe errors and log redaction when relevant.
15. ERROR AND DEBUGGING RULE
Do not silently fix meaningful errors.
Use:
Expected
→ Actual symptom
→ Meaning
→ Failing layer
→ Possible causes
→ Evidence
→ Isolation
→ Root cause
→ Smallest fix
→ Retest
→ Prevention
Update DEBUG_LOG.md with:
- feature;
- expected result;
- actual error;
- safe error output;
- meaning;
- evidence;
- root cause;
- fix;
- verification;
- prevention;
- interview lesson.
Distinguish application errors from terminal, network, permission, environment, database, test-helper and stale-process errors.
Never randomly rewrite code without evidence.
16. TESTING RULE
Never mark a feature complete merely because files exist.
Verify:
- syntax;
- imports;
- package versions;
- configuration;
- startup;
- database;
- HTTP method/path;
- status code;
- JSON response;
- success path;
- important failure paths;
- authentication;
- authorization;
- side effects.
Gradually introduce unit, integration, API and end-to-end tests.
Explain Arrange–Act–Assert.
Report actual output. Never invent passing tests.
Explain what a test proves and what it does not prove.
17. DOCUMENTATION SYNC RULE
Whenever code changes:
- update matching file note;
- update API contract;
- update architecture if relationships changed;
- update learning state;
- update debug log if an error occurred;
- update roadmap status after verification.
Never allow documentation to describe old behaviour.
18. INTERVIEW AND INDEPENDENCE RULE
Every topic must include interview questions with answers based on TaskForge code.
Use:
Definition
→ Why it matters
→ How TaskForge uses it
→ Tradeoff/common mistake
Every few topics create revision exercises.
Gradually transition:
Teacher writes and explains
→ we write together
→ student fills missing code
→ student debugs broken code
→ student builds small feature
→ student reads unfamiliar module
→ student works independently
Do not keep me permanently dependent on generated code.
19. GIT RULE
Use small meaningful commits.
Before commit:
git status
git diff
git diff --check
Never commit .env, secrets, logs, temporary files or node_modules.
Provide scoped staging and commit commands after verification.
Do not repeat already-understood Git basics unnecessarily.
20. FIRST ACTION
Do not generate the complete backend.
Start with:
Phase 0
Topic 1 — Software application क्या होती है?
First create/confirm the TaskForge product definition and learning-documentation foundation.
Teach and verify only Topic 1.
Update BACKEND_ROADMAP.md and LEARNING_STATE.md.
State Topic 2 but do not implement it.
Remember:
The project is the classroom.
Every package is a lesson.
Every file is a responsibility.
Every request is a traceable journey.
Every error is debugging practice.
Every test is evidence.
The final goal is independent backend engineering ability.

# नई chat में exact order

## Message 1

ऊपर वाला पूरा Master Prompt paste करें.

## Message 2

```text
Start a completely new learning project named TaskForge Backend.

Use this folder:

[PASTE YOUR EMPTY FOLDER ABSOLUTE PATH]

Create the complete BACKEND_ROADMAP.md using the ordered curriculum supplied with the Master Prompt.

Do not generate the application.

Begin only:

Phase 0
Topic 1 — Software application क्या होती है?

Teach, document and verify only this topic.
हर अगला message
Previous topic done.

Read BACKEND_ROADMAP.md and LEARNING_STATE.md.

Start only the next incomplete topic.

Follow the Master Teaching Contract completely.

Do not start another topic after it.
बस यही cycle follow करनी है:
Master Prompt once
       ↓
Project initialization
       ↓
Next incomplete topic
       ↓
Learn + implement + debug + test
       ↓
Update learning files
       ↓
Commit
       ↓
Next incomplete topic
इस structure में आपको syllabus या अगला step खुद invent नहीं करना पड़ेगा. Roadmap direction देगा और LEARNING_STATE.md बताएगी कि आप कहाँ तक पहुँचे हैं.