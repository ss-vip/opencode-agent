---
name: devops-runtime
description: Non-blocking cross-OS runtime lifecycle management with detached execution, timeout enforcement, surgical execution rules, and anti-hang safeguards.
---

# DevOps Runtime Skill

Use for:
- dev server startup
- background processes
- runtime lifecycle management
- port verification
- deployment/dev workflows

## Core Rules

- commands must return immediately
- no blocking waits
- no inline sleep chains
- detached execution only
- timeout required for all network checks
- verify PID before port checks
- only terminate tracked PIDs

## Forbidden

```bash
npm run dev && curl localhost:3000
bun dev ; sleep 5
killall node
pkill node
taskkill /IM node.exe
```

## Detached Lifecycle

### Phase N

- spawn detached process
- redirect stdout/stderr
- store PID/port/status
- exit immediately

Persist runtime state:

./temp/active_sessions.json

### Phase N+1

- verify PID exists
- inspect stderr delta
- verify port readiness
- use timeout-protected healthchecks
- never infinite loop

## Timeout Rules

All network operations:

- HTTP
- socket
- IPC
- fetch

must enforce:

2000ms max

## Cross-OS Commands

Windows:
netstat -ano | findstr <port>
taskkill /F /PID <pid>

Unix:
lsof -i :<port> -t
kill -9 <pid>

Only tracked PIDs allowed.

## File Rules

- avoid broad scans
- avoid massive rewrites
- prefer targeted reads
- prefer line-range modifications

## Failure Recovery

1. inspect stderr/log
2. retry once
3. reduce scope
4. log to ./temp/defects.md
5. stop infinite retry

## Verification Scope

- impacted system only
- PID cleanup
- port cleanup
- temp leakage check

## Communication

- concise
- structured
- zh-Hant-TW explanation
- English technical terms
