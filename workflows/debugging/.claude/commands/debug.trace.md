---
description: Trace execution paths through code to identify failure points
displayName: Trace Code
icon: 🔬
---

## Trace Execution Paths

Systematically trace through code to understand execution flow and identify where things go wrong.

---

## Execution Steps

### 1. Load Context

Read incident analysis and observability findings:

```bash
LATEST_INCIDENT=$(ls -td artifacts/incidents/*/ | head -1)
```

Review:
- Hypotheses from observability analysis
- Suspected services/components
- Error messages and stack traces

### 2. Trace Execution with Code Explorer

Use **code-explorer** agent to trace through the code:

```
Task tool with subagent_type: code-explorer
Prompt: "Trace execution paths for this incident:

Incident: [Summary]
Suspected Area: [From observability analysis]
Error Messages: [Specific errors]

Tasks:
1. Find entry point for affected requests
2. Trace execution path from entry to failure point
3. Identify all components in the call chain
4. Examine error handling and retry logic
5. Look for race conditions, resource leaks, or bottlenecks

Return:
- Complete execution path (entry → failure)
- Key files to examine (5-10 files)
- Specific functions where issues likely occur
- Code patterns that could cause observed symptoms"
```

### 3. Examine Identified Code

Read all files identified by the agent:
- Request handlers
- Business logic
- Database queries
- External service calls
- Error handling

Focus on:
- Code changes in recent deploys
- Resource management (connections, memory, threads)
- Error handling and retries
- Synchronization and locking

### 4. Analyze Code Patterns

Look for common failure patterns:

**Resource Leaks:**
- Connections not closed
- Memory not freed
- Threads not released

**Race Conditions:**
- Unsynchronized access to shared state
- Missing locks or barriers
- Improper async handling

**Error Amplification:**
- Retry without backoff
- No circuit breakers
- Error propagation without rate limiting

**Performance Issues:**
- N+1 queries
- Inefficient algorithms
- Missing caching

### 5. Document Trace Analysis

Create detailed trace document showing execution path and findings.

Save to: `artifacts/incidents/$INCIDENT_ID/trace.md`

### 6. Update Progress

```
TodoWrite:
  [✅] Initialize incident investigation
  [✅] Analyze observability data
  [✅] Trace execution paths
  [ ] Diagnose root cause
  [ ] Implement fixes
  [ ] Generate runbook
```

### 7. Present Findings

```
✅ Execution trace complete!

Execution Path:
  [Entry] → [Step 1] → [Step 2] → [FAILURE POINT]

Failure Point: [File:Line]
Cause: [Specific code issue]

Next step: Run `/debug.diagnose` for root cause analysis
```

---

## Examples

**Example: Connection Pool Exhaustion**
```
✅ Execution trace complete!

Execution Path:
  APIController → UserService → DatabaseClient → Connection Pool

Failure Point: DatabaseClient.query() (database.ts:145)
Cause: Connections acquired but never released in error path

Code Issue:
  try {
    conn = pool.acquire()
    result = conn.query(sql)
    return result
  } catch (error) {
    throw error  // BUG: Connection never released!
  }

Next step: Run `/debug.diagnose`
```
