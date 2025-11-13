---
description: Perform root cause analysis and generate comprehensive RCA document
displayName: Diagnose RCA
icon: 🎯
---

## Root Cause Analysis

Synthesize all findings into a comprehensive root cause analysis.

---

## Execution Steps

### 1. Load All Investigation Data

Read all previous analysis:
- Initial incident analysis
- Observability findings
- Execution trace analysis

### 2. Perform RCA with SRE Agent

Use **sre-reliability-engineer** agent for systematic RCA:

```
Task tool with subagent_type: sre-reliability-engineer
Prompt: "Perform root cause analysis for this incident:

Symptoms: [From analysis.md]
Observability Data: [From observability.md]
Code Trace: [From trace.md]

Apply the Five Whys methodology:
1. Why did the incident occur?
2. Why did that happen?
3. Why did that happen?
4. Why did that happen?
5. Why did that happen? (root cause)

Provide:
- Root cause (definitive technical reason)
- Contributing factors
- Why it wasn't caught before production
- Similar incidents to prevent
- Preventive measures"
```

### 3. Document Root Cause Analysis

Create comprehensive RCA document:

```markdown
# Root Cause Analysis: [INCIDENT_ID]

## Executive Summary
[One paragraph: what happened, why, and the fix]

## Incident Details
- **Date**: [date]
- **Duration**: [duration]
- **Severity**: [severity]
- **Impact**: [user/business impact]

## Root Cause
[The definitive technical reason for the incident]

### Five Whys Analysis
1. Why did [symptom] occur?
   → [Answer]
2. Why did [previous answer] happen?
   → [Answer]
3. Why did [previous answer] happen?
   → [Answer]
4. Why did [previous answer] happen?
   → [Answer]
5. Why did [previous answer] happen?
   → **[ROOT CAUSE]**

## Contributing Factors
1. [Factor 1]
2. [Factor 2]
3. [Factor 3]

## Why It Wasn't Detected
- Missing monitoring: [what was missing]
- Testing gaps: [what tests would have caught this]
- Code review: [what review would have caught this]

## Preventive Measures
- [ ] [Immediate fix]
- [ ] [Test addition]
- [ ] [Monitoring addition]
- [ ] [Process improvement]

## Similar Incidents to Prevent
[Other scenarios with similar root causes]
```

Save to: `artifacts/incidents/$INCIDENT_ID/rca.md`

### 4. Present RCA

```
✅ Root cause analysis complete!

Root Cause: [One sentence summary]

Contributing Factors:
  - [Factor 1]
  - [Factor 2]

Prevention:
  - [Measure 1]
  - [Measure 2]

Full RCA: artifacts/incidents/[ID]/rca.md

Next step: Run `/debug.fix` to implement fixes
```

---

## Update Progress

```
TodoWrite:
  [✅] Initialize incident investigation
  [✅] Analyze observability data
  [✅] Trace execution paths
  [✅] Diagnose root cause
  [ ] Implement fixes
  [ ] Generate runbook
```
