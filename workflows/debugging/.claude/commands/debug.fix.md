---
description: Implement fixes based on root cause analysis
displayName: Implement Fix
icon: 🔧
---

## Implement Fixes

Implement fixes for the root cause and contributing factors.

---

## Execution Steps

### 1. Load RCA

Read root cause analysis to understand what needs to be fixed.

### 2. Implement Fixes Using TDD

Use **tdd-developer** agent to implement fixes:

```
Task tool with subagent_type: tdd-developer
Prompt: "Implement fixes for this incident:

Root Cause: [from rca.md]
Files to Fix: [from trace.md]

Using TDD:
1. Write tests that would have caught this issue
2. Implement fix to pass tests
3. Refactor for reliability

Also implement:
- Error handling improvements
- Resource cleanup (if applicable)
- Circuit breakers (if applicable)
- Retry logic improvements (if applicable)"
```

### 3. Add Monitoring and Alerts

Implement monitoring to detect similar issues:

```bash
# Add metrics, logging, tracing as appropriate
# Examples:
# - Add counter for connection pool exhaustion
# - Add histogram for query duration
# - Add trace spans for critical paths
```

### 4. Review with Code Reviewer

Use **code-reviewer** agent to validate fixes:

```
Task tool with subagent_type: code-reviewer
Prompt: "Review these incident fixes:

Root Cause: [summary]
Fixes Implemented: [list]

Check:
- Does it actually fix the root cause?
- Are there any edge cases not covered?
- Is error handling robust?
- Are there performance implications?
- Will this prevent recurrence?"
```

### 5. Document Implementation

Create fix documentation.

Save to: `artifacts/incidents/$INCIDENT_ID/fix.md`

### 6. Present Fix Summary

```
✅ Fixes implemented!

Changes:
  - [file 1]: [change description]
  - [file 2]: [change description]

Tests Added:
  - [test 1]
  - [test 2]

Monitoring Added:
  - [metric/alert 1]
  - [metric/alert 2]

Next step: Run `/debug.document` to generate runbook
```

---

## Update Progress

```
TodoWrite:
  [✅] Initialize incident investigation
  [✅] Analyze observability data
  [✅] Trace execution paths
  [✅] Diagnose root cause
  [✅] Implement fixes
  [ ] Generate runbook
```
