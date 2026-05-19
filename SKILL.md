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

```text
./temp/active_sessions.json
```

### Phase N+1

- verify PID exists
- inspect stderr delta
- verify port readiness
- use timeout-protected healthchecks
- never infinite loop

## Timeout Rules

All:
- HTTP
- socket
- IPC
- fetch

require explicit timeout.

Maximum:

```text
2000ms
```

## Cross-OS Commands

### Windows

Port check:

```powershell
netstat -ano | findstr <port>
```

Kill process:

```powershell
taskkill /F /PID <pid>
```

### Unix

Port check:

```bash
lsof -i :<port> -t
```

Kill process:

```bash
kill -9 <pid>
```

Only terminate tracked PIDs.

## Sandbox Rules

Temporary/debug scripts:
- must stay inside `./temp/`
- should include timestamps
- must be deleted after success

## File Rules

- avoid broad recursive scans
- avoid massive rewrites
- prefer targeted reads
- prefer line-range modifications

## Failure Recovery

If execution fails:

1. inspect stderr/log delta
2. retry once only
3. reduce execution scope
4. record failure into:

```text
./temp/defects.md
```

5. stop infinite retry loops

## Verification

Verify impacted scope only:
- relevant tests
- relevant lint/typecheck
- PID cleanup
- orphan port cleanup
- sandbox leakage

## Communication

- concise
- structured
- low-token
- zh-Hant-TW explanations
- English technical terminology
