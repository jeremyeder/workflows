---
description: Gather and analyze observability data (metrics, logs, traces)
displayName: Analyze Data
icon: 📊
---

## Analyze Observability Data

Systematically gather and analyze metrics, logs, and traces to understand the incident.

---

## Execution Steps

### 1. Load Incident Context

Read the incident analysis to understand what we're investigating:

```bash
# Find the most recent incident
LATEST_INCIDENT=$(ls -td artifacts/incidents/*/ | head -1)
INCIDENT_ID=$(basename "$LATEST_INCIDENT")
```

If no incident found:
```
Error: No active incident investigation found.
Please run `/debug.start` first to initialize the investigation.
```

Read `artifacts/incidents/$INCIDENT_ID/analysis.md` to understand:
- Symptoms and timeframe
- Affected systems
- Error messages
- Initial hypothesis

### 2. Gather Metrics Data

Use the **sre-reliability-engineer** agent to analyze metrics:

```
Task tool with subagent_type: sre-reliability-engineer
Prompt: "Analyze this incident's metrics and observability data:

Incident: [Summary from analysis.md]
Timeframe: [Start time] to now
Affected Systems: [Systems list]

Tasks:
1. Identify metric anomalies (CPU, memory, latency, error rates)
2. Correlate metric spikes with incident timeline
3. Check for resource exhaustion patterns
4. Analyze traffic patterns and load distribution
5. Identify cascading failures if present

OBSERVABILITY INTEGRATION PLACEHOLDER:
# When observability systems are integrated, query:
# - Prometheus/Grafana for system metrics
# - APM tools for application performance
# - Infrastructure monitoring for resource usage

For now, provide guidance on:
- What metrics to check
- How to correlate metrics with symptoms
- Where to find metric data in the project

Return a summary of metric analysis and areas of concern."
```

### 3. Gather Log Data

Analyze relevant logs:

```bash
# PLACEHOLDER: Real log aggregation integration
# When integrated with ELK/Splunk/Loki:
# - Query logs for timeframe
# - Filter by affected services
# - Search for error patterns

# For now: Guide user to gather logs
echo "Log Analysis Needed:"
echo "  1. Gather logs from affected services"
echo "  2. Focus on timeframe: [INCIDENT_START] to now"
echo "  3. Search for: error, exception, timeout, 500, failed"
echo ""
echo "Commands to run (example):"
echo "  kubectl logs -l app=api-service --since=2h | grep -i error"
echo "  journalctl -u myservice --since '2 hours ago' | grep -E 'error|fail'"
```

If user provides logs, analyze them:
- Extract error patterns
- Identify unique errors vs repeated errors
- Find correlation with incident timeline
- Look for cascading failures

### 4. Gather Trace Data

Analyze distributed traces:

```bash
# PLACEHOLDER: Distributed tracing integration
# When integrated with Jaeger/Tempo/Zipkin:
# - Query traces for timeframe
# - Filter by error status
# - Analyze slow traces

# For now: Guide user to gather traces
echo "Trace Analysis Needed:"
echo "  1. Access your tracing system (Jaeger/Tempo/Zipkin)"
echo "  2. Search for traces in timeframe: [INCIDENT_START] to now"
echo "  3. Filter by: error=true OR duration > [threshold]"
echo "  4. Look for: slow spans, failed requests, timeout patterns"
```

If user provides traces, analyze them:
- Identify slow services in trace
- Find where requests are failing
- Check for timeout cascades
- Examine retry patterns

### 5. Correlate Data Sources

Synthesize findings from all observability sources:

**Correlation Analysis:**
- Timeline correlation: When did metrics spike? When did logs show errors?
- Service correlation: Which services show problems? In what order?
- Pattern correlation: Do errors follow a pattern? (time-based, load-based, etc.)

Create correlation matrix:
```markdown
| Time  | Metrics           | Logs                 | Traces            |
|-------|-------------------|----------------------|-------------------|
| 15:30 | Deploy completed  | Normal activity      | Normal latency    |
| 15:35 | CPU spike to 80%  | Connection warnings  | Latency increase  |
| 15:40 | Memory exhaustion | OOM errors start     | Request failures  |
| 15:45 | Service restarts  | Connection pool full | Cascading timeout |
```

### 6. Document Observability Findings

Create comprehensive observability document:

```markdown
# Observability Analysis: [INCIDENT_ID]

## Data Collection Summary

### Timeframe Analyzed
- **Start**: [incident start time]
- **End**: [current time]
- **Duration**: [duration]

### Data Sources
- [✅/⚠️] Metrics (Prometheus/Grafana/etc.)
- [✅/⚠️] Logs (ELK/Splunk/etc.)
- [✅/⚠️] Traces (Jaeger/Tempo/etc.)
- [✅/⚠️] Infrastructure monitoring

---

## Metrics Analysis

### Key Metric Anomalies

**Service: [service-name]**
- **CPU Usage**: [normal] → [incident] ([percentage] increase)
- **Memory Usage**: [normal] → [incident] ([percentage] increase)
- **Request Rate**: [normal] → [incident] ([percentage] change)
- **Error Rate**: [normal] → [incident] ([percentage] increase)
- **Latency P50/P95/P99**: [baseline] → [incident]

**Database: [db-name]**
- **Connection Pool**: [normal] → [incident]
- **Query Time**: [normal] → [incident]
- **Lock Wait Time**: [normal] → [incident]

### Metric Correlation
[Graph or description of how metrics correlate with incident timeline]

---

## Log Analysis

### Error Patterns Found

**Most Common Errors** (top 5):
1. `[error message]` - [count] occurrences
2. `[error message]` - [count] occurrences
3. `[error message]` - [count] occurrences

**Error Timeline**:
```
15:35 - First warning: [message]
15:38 - Errors begin: [message]
15:42 - Error rate spikes: [message]
15:45 - Cascading failures: [message]
```

**Unique vs Repeated**:
- Unique error types: [count]
- Total error occurrences: [count]
- Most repeated: `[error]` ([count] times)

### Stack Traces
```
[Most relevant stack trace]
```

---

## Trace Analysis

### Slow Traces Identified

**Top 3 Slowest Operations**:
1. `GET /api/users` - [duration]ms ([normal baseline]ms) - [service]
2. `POST /api/orders` - [duration]ms ([normal baseline]ms) - [service]
3. `GET /api/products` - [duration]ms ([normal baseline]ms) - [service]

### Failed Traces

**Failure Points**:
- [service] → [service]: Timeout after [duration]ms
- [service] → [database]: Connection refused
- [service] → [cache]: Connection pool exhausted

### Trace Patterns
[Description of patterns observed in traces]

---

## Correlation Analysis

### Timeline Correlation

| Time  | Event                    | Metrics              | Logs                    | Traces              |
|-------|--------------------------|----------------------|-------------------------|---------------------|
| 15:30 | [Event]                  | [Metric change]      | [Log observation]       | [Trace observation] |
| 15:35 | [Event]                  | [Metric change]      | [Log observation]       | [Trace observation] |

### Service Correlation

**Affected Services** (in order):
1. [Service A] - First to show issues
2. [Service B] - Cascaded from A
3. [Service C] - Cascaded from B

**Dependency Chain**:
```
[Service A] → [Service B] → [Service C] → [Database]
    ↓             ↓             ↓
  Timeout     Connection     Pool
              exhaustion   exhaustion
```

---

## Key Findings

### Primary Observations
1. [Key observation 1]
2. [Key observation 2]
3. [Key observation 3]

### Red Flags
- 🚩 [Critical finding]
- 🚩 [Critical finding]

### Patterns Identified
- [Pattern 1]
- [Pattern 2]

---

## Hypotheses Generated

Based on observability data:

1. **Hypothesis 1**: [Description]
   - Supporting evidence: [Data points]
   - Confidence: [High/Medium/Low]

2. **Hypothesis 2**: [Description]
   - Supporting evidence: [Data points]
   - Confidence: [High/Medium/Low]

3. **Hypothesis 3**: [Description]
   - Supporting evidence: [Data points]
   - Confidence: [High/Medium/Low]

---

## Recommended Next Steps

1. [ ] Trace execution path through [service/component]
2. [ ] Examine [specific code area] for issues
3. [ ] Check [configuration/resource] settings
4. [ ] Test hypothesis: [specific test]

---

## Data Artifacts

Saved data files:
- Metrics: `artifacts/incidents/[ID]/metrics/[files]`
- Logs: `artifacts/incidents/[ID]/logs/[files]`
- Traces: `artifacts/incidents/[ID]/traces/[files]`

---

## Notes
[Any additional observations or context]
```

Save to: `artifacts/incidents/$INCIDENT_ID/observability.md`

### 7. Update Investigation Progress

```
TodoWrite:
  [✅] Initialize incident investigation
  [✅] Analyze observability data
  [ ] Trace execution paths
  [ ] Diagnose root cause
  [ ] Implement fixes
  [ ] Generate runbook
  [ ] Close incident
```

### 8. Present Analysis Summary

```
✅ Observability analysis complete!

Key Findings:
  - [Primary metric anomaly]
  - [Primary log pattern]
  - [Primary trace issue]

Top Hypotheses:
  1. [Hypothesis with confidence level]
  2. [Hypothesis with confidence level]

Correlation: [Brief correlation summary]

Full analysis saved to:
  artifacts/incidents/[INCIDENT_ID]/observability.md

Next step: Run `/debug.trace` to trace execution paths through the code and validate hypotheses.
```

---

## Observability Integration Notes

### For Prometheus/Grafana
```bash
# Example queries to implement when integrated:
# CPU usage spike detection
rate(container_cpu_usage_seconds_total[5m]) > 0.8

# Memory exhaustion detection
container_memory_usage_bytes / container_spec_memory_limit_bytes > 0.9

# Error rate spike
rate(http_requests_total{status=~"5.."}[5m]) > threshold
```

### For ELK/Splunk
```bash
# Example log queries to implement:
# Error pattern search
index=app-logs level=ERROR | stats count by message

# Timeline analysis
index=app-logs | timechart count by level

# Specific error investigation
index=app-logs "Connection pool exhausted" | table _time, host, message
```

### For Jaeger/Tempo
```bash
# Example trace queries to implement:
# Find slow traces
duration > 2s AND error=true

# Service dependency analysis
service=api-service | dependencyGraph

# Error traces
tag.error=true | top operation
```

---

## Examples

**Example 1: Clear Metric Spike**
```
✅ Observability analysis complete!

Key Findings:
  - Database CPU spiked from 20% to 95% at 15:35
  - Connection pool exhausted at 15:40
  - Query time increased 10x (200ms → 2000ms)

Top Hypotheses:
  1. Slow query introduced in recent deploy (High confidence)
  2. Database index missing for new query pattern (Medium)

Correlation: Metric spike correlates exactly with deploy at 15:30

Next step: Run `/debug.trace` to examine queries in recent deploy
```

**Example 2: Cascading Failure**
```
✅ Observability analysis complete!

Key Findings:
  - Service A timeout → Service B retry storm → Service C overload
  - Error propagation: A (15:35) → B (15:38) → C (15:42)
  - All services showing connection pool exhaustion

Top Hypotheses:
  1. Service A slow response causing cascading backpressure (High)
  2. Retry logic amplifying load (High)
  3. No circuit breaker preventing cascade (Medium)

Next step: Run `/debug.trace` to examine Service A request handling
```

---

## Error Handling

**No Incident Found:**
```
Error: No active incident investigation.
Please run `/debug.start` first.
```

**Observability Systems Unavailable:**
```
⚠️  Note: Observability integrations are not configured.

Please manually gather:
  - Metrics from your monitoring system
  - Logs from affected services
  - Traces from your tracing system

Once gathered, I'll analyze the data.
```
