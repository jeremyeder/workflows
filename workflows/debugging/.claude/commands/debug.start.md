---
description: Initialize incident debugging session and gather initial information
displayName: Start Debugging
icon: 🔍
---

## Start Debugging Session

Initialize a comprehensive debugging session for incident analysis and resolution.

### User Input

```text
$ARGUMENTS
```

Expected input: Incident description including symptoms, error messages, affected systems, timeframe, and any other relevant context.

---

## Execution Steps

### 1. Validate and Parse Input

Check that sufficient information was provided:

**Required Information** (prompt if missing):
- What is happening? (symptoms)
- When did it start? (timeframe)
- What systems are affected? (scope)

**Helpful Information** (request if not provided):
- Error messages or stack traces
- User impact (how many users affected)
- Recent changes (deployments, config changes)
- Previous similar incidents

If input is insufficient, ask specific questions using AskUserQuestion.

### 2. Create Incident Workspace

Set up a dedicated workspace for this debugging session:

```bash
# Generate incident ID (timestamp-based)
INCIDENT_ID=$(date +%Y%m%d-%H%M%S)

# Create directories
mkdir -p "artifacts/incidents/$INCIDENT_ID"

# Create subdirectories for organization
mkdir -p "artifacts/incidents/$INCIDENT_ID/logs"
mkdir -p "artifacts/incidents/$INCIDENT_ID/metrics"
mkdir -p "artifacts/incidents/$INCIDENT_ID/traces"
```

### 3. Document Initial State

Create initial incident analysis document:

```markdown
# Incident Analysis: [INCIDENT_ID]

## Incident Overview

### Symptoms
[What users/systems are experiencing]

### Timeframe
- **Started**: [When the incident began]
- **Detected**: [When we became aware]
- **Current Status**: [Ongoing / Mitigated / Resolved]

### Affected Systems
[List of affected services, components, or infrastructure]

### Impact
- **Users Affected**: [Number or percentage]
- **Severity**: [Critical / High / Medium / Low]
- **Business Impact**: [Revenue, SLA, reputation, etc.]

### Error Messages
```
[Paste any error messages, stack traces, or logs]
```

### Recent Changes
[Deployments, configuration changes, infrastructure changes, etc.]

### Timeline
- [HH:MM] - [Event description]
- [HH:MM] - [Event description]

## Initial Hypothesis
[Your initial thoughts on what might be wrong]

## Investigation Plan
1. [ ] Analyze observability data (metrics, logs, traces)
2. [ ] Trace execution path through affected code
3. [ ] Identify root cause
4. [ ] Implement fix
5. [ ] Generate runbook for future incidents

## Notes
[Any additional context or observations]
```

Save to: `artifacts/incidents/$INCIDENT_ID/analysis.md`

### 4. Set Up Investigation Tracking

Use TodoWrite to track the investigation:

```
TodoWrite:
  [✅] Initialize incident investigation
  [ ] Analyze observability data
  [ ] Trace execution paths
  [ ] Diagnose root cause
  [ ] Implement fixes
  [ ] Generate runbook
  [ ] Close incident
```

### 5. Quick Triage Questions

Ask essential triage questions if not already answered:

**Severity Assessment:**
- Is this a production outage? (Yes/No)
- Are users currently affected? (Yes/No)
- Is data at risk? (Yes/No)
- Do we need to engage on-call teams? (Yes/No)

**Scope Assessment:**
- Percentage of users affected? (All / Many / Some / Few)
- Geographic scope? (Global / Regional / Single datacenter)
- Service scope? (All services / Single service / Component)

**Mitigation:**
- Is there a quick mitigation available? (Rollback, feature flag, etc.)
- Should we mitigate now or investigate first?

Use AskUserQuestion for quick triage if critical information is missing.

### 6. Present Investigation Start

Provide clear summary and next steps:

```
🚨 Incident Investigation Started

Incident ID: [INCIDENT_ID]
Severity: [CRITICAL/HIGH/MEDIUM/LOW]
Status: [Investigating]

Symptoms: [Brief description]
Affected: [Systems and users]
Started: [Timeframe]

Investigation workspace created at:
  artifacts/incidents/[INCIDENT_ID]/

Investigation plan:
  1. Gather observability data
  2. Trace execution paths
  3. Identify root cause
  4. Implement fix
  5. Generate runbook

Next step: Run `/debug.analyze` to gather observability data and begin investigation.

⚠️  If this is a critical production incident, consider implementing immediate mitigation (rollback, feature flag) while investigating root cause.
```

---

## Error Handling

**No Input Provided:**
```
Error: No incident description provided.

Please provide incident details:
  /debug.start [incident description]

Example:
  /debug.start API returning 500 errors for 20% of requests since 14:30 UTC. Error: "Database connection timeout"

Required information:
  - What is happening? (symptoms)
  - When did it start? (timeframe)
  - What systems are affected? (scope)
```

**Insufficient Information:**
```
⚠️  Need more information to effectively investigate.

You provided: [summary]

Please also provide:
  - [ ] Error messages or stack traces
  - [ ] Timeframe (when did it start?)
  - [ ] User impact (how many affected?)
  - [ ] Recent changes (deployments, etc.)

You can add this information now or proceed and add it during investigation.
```

---

## Examples

**Example 1: Production API Outage**
```
User: /debug.start API returning 500 errors for 50% of requests since 15:45 UTC. Error: "Connection pool exhausted". Recent deploy at 15:30.

Agent: 🚨 Incident Investigation Started

Incident ID: 20250113-154500
Severity: CRITICAL
Status: Investigating

Symptoms: API 500 errors (50% of requests)
Error: Connection pool exhausted
Affected: Production API service
Started: 15:45 UTC (15 minutes ago)
Recent Change: Deploy at 15:30 UTC

⚠️  CRITICAL INCIDENT - Consider immediate rollback of 15:30 deploy

Investigation workspace: artifacts/incidents/20250113-154500/

Next step: Run `/debug.analyze` to examine metrics and logs

Do you want to rollback immediately, or investigate first?
```

**Example 2: Gradual Performance Degradation**
```
User: /debug.start Application response time increased from 200ms to 2000ms over the last 2 hours. No recent deploys. Database CPU at 80%.

Agent: 🚨 Incident Investigation Started

Incident ID: 20250113-160000
Severity: HIGH
Status: Investigating

Symptoms: 10x response time degradation
Affected: Application performance
Started: ~2 hours ago (gradual)
Observation: Database CPU at 80%

Investigation plan:
  1. Analyze database metrics and slow queries
  2. Check for data growth or query pattern changes
  3. Examine application code for N+1 queries
  4. Identify root cause
  5. Implement optimization

Next step: Run `/debug.analyze` to gather database metrics

No immediate mitigation needed - service is degraded but functional.
```

**Example 3: Intermittent Errors**
```
User: /debug.start Users reporting intermittent login failures. No pattern. Error logs show "Invalid session token" randomly.

Agent: 🚨 Incident Investigation Started

Incident ID: 20250113-161500
Severity: MEDIUM
Status: Investigating

Symptoms: Intermittent login failures
Error: Invalid session token
Pattern: Random (no clear pattern)
User Impact: Unknown percentage

⚠️  Intermittent issues are challenging - will need thorough tracing.

Investigation focus:
  1. Analyze session management code
  2. Check for race conditions
  3. Examine distributed cache consistency
  4. Review recent session changes

Next step: Run `/debug.analyze` to gather session-related logs

This may require detailed code tracing to reproduce the issue.
```

---

## Integration Points (Placeholders)

This command sets up integration hooks for observability systems. Actual integration is handled in `/debug.analyze`.

**Metrics Systems** (Prometheus, Grafana, Datadog):
```bash
# Placeholder: Set timeframe for metric queries
INCIDENT_START="[timestamp]"
INCIDENT_END="now"
```

**Log Aggregation** (ELK, Splunk, Loki):
```bash
# Placeholder: Set log query parameters
LOG_TIMEFRAME="[start]-[end]"
LOG_FILTERS="error OR 500 OR timeout"
```

**Distributed Tracing** (Jaeger, Tempo, Zipkin):
```bash
# Placeholder: Set trace search parameters
TRACE_TIMEFRAME="[start]-[end]"
TRACE_FILTERS="error=true"
```

These are configured but not executed in this command - `/debug.analyze` will use them.

---

## Best Practices

1. **Act Fast for Critical Incidents**: If production is down, consider immediate mitigation (rollback, circuit breaker) before deep investigation
2. **Document Everything**: Timeline, observations, hypotheses - all documented in real-time
3. **Think Systematically**: Follow the investigation plan rather than random changes
4. **Correlate with Changes**: Always check recent deploys, config changes, traffic patterns
5. **Communicate**: Keep stakeholders informed of progress and ETA

---

## Notes

- This command focuses on setup and information gathering
- Actual investigation happens in subsequent commands
- All artifacts are saved to the incident workspace
- The workspace is organized for easy navigation during high-stress incidents
- Timeline and notes are continuously updated throughout investigation
