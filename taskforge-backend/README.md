# TaskForge Backend

TaskForge ek evolving team project, task aur issue management backend learning project hai.

## Current status

Phase 2 — Git aur repository foundation complete hai. Backend JavaScript, Node.js, npm aur application
runtime abhi initialize nahi hue hain.

## Learning goal

Project ko step-by-step build karke request flow, database work, authentication, authorization,
testing, security, deployment aur system design independently samajhna hai.

## Current files

```text
taskforge-backend/
|-- .env.example   # Safe environment-variable names; private values empty hain
|-- .gitignore     # Dependencies, secrets, logs aur generated output ignore rules
`-- README.md      # Project purpose aur current foundation state
```

## Repository boundary

`taskforge-backend/` current parent `Practicle` Git repository ka child folder hai. Is folder ka own
`.git` directory nahi hai, isliye accidental nested repository create nahi hui.

## Secret safety

- Real credentials source code ya Git history mein commit nahi karne hain.
- Local private values future ignored `.env` file mein hongi.
- `.env.example` mein केवल safe defaults, empty values aur variable names rahenge.
- Secret expose hone par credential ko revoke/rotate karna first priority hai.

## Run instructions

Abhi application entry file aur `package.json` exist nahi karte, isliye run command intentionally
available nahi hai. Node application later ordered curriculum mein initialize hogi.

## Next learning step

Phase 3 — Backend JavaScript prerequisites, Topic 58: Statement aur expression.
