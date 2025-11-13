---
description: Generate comprehensive runbook for future incidents
displayName: Create Runbook
icon: 📚
---

## Generate Runbook

Create a comprehensive runbook for handling similar incidents in the future.

---

## Execution Steps

### 1. Load All Investigation Artifacts

Read all analysis documents to create comprehensive runbook.

### 2. Generate Runbook

Create structured runbook document:

```markdown
# Runbook: [Incident Type]

*Generated from incident [INCIDENT_ID]*

## Quick Reference

**Symptoms**: [Brief description]
**Severity**: [Typical severity]
**MTTR**: [Mean time to resolution]

### Quick Mitigation
[Immediate steps to stop the bleeding]

---

## Detection

### Alerts
- Alert: [alert name]
  - Trigger: [condition]
  - Action: Use this runbook

### Symptoms
- [Symptom 1]: [How users/systems experience it]
- [Symptom 2]: [Description]

### Metrics to Check
- [Metric 1]: Normal [X], Problem [Y]
- [Metric 2]: Normal [X], Problem [Y]

---

## Investigation

### Step 1: Verify the Incident
```bash
# Check [metric/log/service]
[command to run]
```

Expected: [what you should see if this is the issue]

### Step 2: Identify Scope
```bash
# Check affected services
[commands]
```

Questions to answer:
- [ ] How many users affected?
- [ ] Which services are impacted?
- [ ] Is data at risk?

### Step 3: Check Recent Changes
- Deployments in last [timeframe]
- Configuration changes
- Traffic pattern changes

---

## Mitigation

### Immediate Actions (Stop the Bleeding)

**Option 1: Rollback** (if recent deploy)
```bash
[rollback commands]
```

**Option 2: Circuit Breaker** (if dependency failing)
```bash
[circuit breaker commands/feature flag]
```

**Option 3: Scale** (if resource exhaustion)
```bash
[scaling commands]
```

### Validation
After mitigation, verify:
```bash
# Check metrics return to normal
[validation commands]
```

---

## Resolution

### Permanent Fix
1. [Step 1 description]
   ```bash
   [commands]
   ```

2. [Step 2 description]
   ```bash
   [commands]
   ```

3. [Step 3 description]
   ```bash
   [commands]
   ```

### Testing
Before deploying fix:
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Load tests show no regression
- [ ] Fix deployed to staging and validated

---

## Root Cause

**Technical Cause**: [Brief technical explanation]

**Why It Happened**: [Contributing factors]

**Why It Wasn't Caught**: [Testing/monitoring gaps]

---

## Prevention

### Code Changes
- [Change 1]: Implemented in [PR/commit]
- [Change 2]: Implemented in [PR/commit]

### Monitoring Additions
- [Metric/Alert 1]: [What it monitors]
- [Metric/Alert 2]: [What it monitors]

### Process Improvements
- [Process 1]: [Description]
- [Process 2]: [Description]

---

## Communication

### Incident Response Team
- On-call: [Pagerduty/Slack channel]
- Escalation: [Manager/senior engineer]
- Stakeholders: [Who to notify]

### Status Updates
- Update [stakeholder channel] every [timeframe]
- Include: Current status, ETA, user impact
- Template: [status update template]

### Post-Incident
- RCA: [link to full RCA]
- Postmortem meeting: [when scheduled]
- Action items: [tracking system]

---

## Related

### Similar Incidents
- [Incident ID]: [Brief description and link]
- [Incident ID]: [Brief description and link]

### Related Runbooks
- [Runbook name]: [When to use]
- [Runbook name]: [When to use]

### Documentation
- [System architecture docs]
- [Deployment process docs]
- [Monitoring dashboard links]

---

## Revision History

- [Date]: Initial version (from incident [ID])
- [Date]: [Update description]

---

## Testing This Runbook

To validate this runbook works:

1. **Simulate the incident** (in staging/test)
   ```bash
   [commands to simulate]
   ```

2. **Follow the runbook** step-by-step

3. **Verify** it resolves the simulated incident

4. **Update** runbook based on learnings

**Last Tested**: [Date]
**Test Result**: [Pass/Fail/Needs Update]
```

Save to: `artifacts/incidents/$INCIDENT_ID/runbook.md`

Also save to project's runbook directory (if exists):
```bash
# If project has runbook directory
if [ -d "docs/runbooks" ] || [ -d "runbooks" ]; then
  cp artifacts/incidents/$INCIDENT_ID/runbook.md [runbook-dir]/incident-[type].md
fi
```

### 3. Complete Investigation

```
TodoWrite:
  [✅] Initialize incident investigation
  [✅] Analyze observability data
  [✅] Trace execution paths
  [✅] Diagnose root cause
  [✅] Implement fixes
  [✅] Generate runbook
  [✅] Close incident
```

### 4. Present Final Summary

```
✅ Incident investigation complete!

Incident ID: [INCIDENT_ID]
Duration: [total investigation time]
Status: Resolved

Deliverables:
  ✅ Root Cause Analysis
  ✅ Fix Implementation
  ✅ Comprehensive Runbook
  ✅ Monitoring Improvements

Artifacts saved to:
  artifacts/incidents/[INCIDENT_ID]/

Runbook: [location]

Recommended Next Steps:
  1. Review RCA with team
  2. Schedule postmortem meeting
  3. Track action items to completion
  4. Test runbook in staging environment
  5. Share learnings with broader team

Great work on resolving this incident! 🎉
```

---

## Examples

**Example: Database Connection Pool Runbook**
```
✅ Runbook generated!

Created: "Database Connection Pool Exhaustion" runbook

Quick Mitigation:
  1. Scale database connection pool
  2. Restart affected services
  3. Monitor recovery

Detection Alerts Added:
  - connection_pool_usage > 90%
  - connection_acquisition_time > 5s

Runbook saved to:
  - artifacts/incidents/20250113-154500/runbook.md
  - docs/runbooks/database-connection-pool-exhaustion.md

Ready for team review and testing!
```
