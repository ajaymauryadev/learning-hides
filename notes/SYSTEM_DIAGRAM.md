# TaskForge High-Level System Diagram

## Status and legend

Yeh **planned conceptual architecture** hai. Solid arrows expected core interaction
dikhate hain; dashed arrow later/optional integration dikhata hai. Current application
code, API, database and external services implement nahi hue.

## System context

```mermaid
flowchart LR
    Actor[TaskForge user or system administrator]
    Client[Client: future web/mobile UI or API testing tool]
    API[TaskForge API boundary]
    Backend[TaskForge modular-monolith backend]
    DB[(Future persistent database)]
    External[Future external services]

    Actor -->|uses| Client
    Client -->|request| API
    API -->|validated operation| Backend
    Backend -->|read/write allowed data| DB
    DB -->|data result/error| Backend
    Backend -.->|email, files or realtime later| External
    Backend -->|safe result/error| API
    API -->|response| Client
    Client -->|visible outcome| Actor
```

## One operation round trip

```mermaid
sequenceDiagram
    actor User
    participant Client as TaskForge client
    participant API as API boundary
    participant Backend as Backend rules
    participant DB as Database

    User->>Client: Initiates product goal
    Client->>API: Sends request and input
    API->>Backend: Passes accepted operation
    Backend->>Backend: Validate identity, permission and business rules
    Backend->>DB: Perform allowed data operation
    DB-->>Backend: Return data result or error
    Backend-->>API: Produce safe success or failure
    API-->>Client: Return response
    Client-->>User: Show meaningful outcome
```

## Responsibilities

- Actor: product goal initiate karta hai.
- Client: input collect, request send, response interpret/display karta hai.
- API boundary: supported operations and communication expectations expose karegi.
- Backend: trusted validation, identity, permissions, rules and coordination karega.
- Database: persistent data store/read/change karega.
- External services: email/file/realtime capability later justified need par.

## Trust boundary

Client user-controlled ho sakta hai, isliye backend client ke validation/hidden
buttons ko trust nahi karega. Protected rule backend mein independently enforce hoga.

