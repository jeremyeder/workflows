# Debugging Workflow

Comprehensive incident debugging workflow for systematic root cause analysis, fix implementation, and runbook generation. Engineering-focused with observability integration.

## Purpose

This workflow provides a structured approach to debugging production incidents by:
- Systematically analyzing observability data (metrics, logs, traces)
- Tracing execution paths through code
- Identifying root causes using proven methodologies (Five Whys)
- Implementing reliable fixes with tests
- Generating comprehensive runbooks for future incidents

This is the **engineering-side equivalent** of the "plan a feature" workflow - comprehensive and thorough.

---

## Quick Start

```bash
# 1. Start investigation
/debug.start API returning 500 errors since 15:45 UTC. Error: "Connection pool exhausted"

# 2. Analyze observability data
/debug.analyze

# 3. Trace code execution
/debug.trace

# 4. Perform root cause analysis
/debug.diagnose

# 5. Implement fixes
/debug.fix

# 6. Generate runbook
/debug.document
```

---

## Workflow Overview

```
/debug.start      → Initialize investigation, document symptoms
      ↓
/debug.analyze    → Gather metrics, logs, traces
      ↓
/debug.trace      → Trace code execution paths
      ↓
/debug.diagnose   → Root cause analysis (Five Whys)
      ↓
/debug.fix        → Implement fixes + tests + monitoring
      ↓
/debug.document   → Generate comprehensive runbook
```

---

## Commands

### /debug.start

Initialize incident investigation and set up workspace.

**Input**: Incident description with symptoms, timeframe, affected systems

**Creates**:
- Incident workspace
- Initial analysis document
- Investigation timeline
- Triage assessment

**Output**: `artifacts/incidents/{id}/analysis.md`

---

### /debug.analyze

Gather and analyze observability data from metrics, logs, and traces.

**Uses**: sre-reliability-engineer agent

**Analyzes**:
- Metrics (CPU, memory, latency, errors)
- Logs (error patterns, stack traces)
- Traces (slow operations, failures)
- Correlations across data sources

**Output**: `artifacts/incidents/{id}/observability.md`

**Note**: Includes placeholders for Prometheus, ELK, Jaeger integration

---

### /debug.trace

Trace execution paths through code to identify failure points.

**Uses**: code-explorer agent

**Identifies**:
- Entry points and failure points
- Complete execution path
- Resource management issues
- Race conditions or bottlenecks
- Error handling gaps

**Output**: `artifacts/incidents/{id}/trace.md`

---

### /debug.diagnose

Perform systematic root cause analysis.

**Uses**: sre-reliability-engineer agent

**Applies**:
- Five Whys methodology
- Contributing factors analysis
- Detection gap analysis
- Prevention measures

**Output**: `artifacts/incidents/{id}/rca.md`

---

### /debug.fix

Implement fixes based on root cause analysis.

**Uses**: tdd-developer, code-reviewer agents

**Implements**:
- Code fixes (with tests)
- Monitoring and alerts
- Error handling improvements
- Resource management fixes

**Output**: `artifacts/incidents/{id}/fix.md`

---

### /debug.document

Generate comprehensive runbook for future incidents.

**Creates**:
- Quick reference guide
- Detection and symptoms
- Investigation steps
- Mitigation procedures
- Permanent resolution
- Prevention measures
- Communication templates

**Output**: `artifacts/incidents/{id}/runbook.md`

---

## Agents Used

### sre-reliability-engineer
- **Used in**: /debug.analyze, /debug.diagnose
- **Purpose**: Incident analysis, metrics correlation, root cause analysis
- **Expertise**: SRE methodologies, observability, system reliability

### code-explorer
- **Used in**: /debug.trace
- **Purpose**: Trace execution paths, identify code issues
- **Expertise**: Code analysis, architecture understanding

### tdd-developer
- **Used in**: /debug.fix
- **Purpose**: Implement fixes with test-first approach
- **Expertise**: Test-driven development, bug fixing

### code-reviewer
- **Used in**: /debug.fix
- **Purpose**: Validate fix quality and completeness
- **Expertise**: Code review, quality assurance

---

## Observability Integration

The workflow includes placeholder integration points for:

### Metrics Systems
- Prometheus
- Grafana
- Datadog
- Custom metrics platforms

### Log Aggregation
- ELK Stack (Elasticsearch, Logstash, Kibana)
- Splunk
- Loki
- Custom log systems

### Distributed Tracing
- Jaeger
- Tempo
- Zipkin
- Custom tracing systems

**Current Status**: Placeholders with example queries and integration guidance

**To Implement**: Add API clients and query logic in `/debug.analyze` command

---

## Outputs

All artifacts saved to `artifacts/incidents/{incident-id}/`:

```
artifacts/incidents/20250113-154500/
├── analysis.md          # Initial incident analysis
├── observability.md     # Metrics, logs, traces analysis
├── trace.md            # Code execution trace
├── rca.md              # Root cause analysis
├── fix.md              # Fix implementation details
├── runbook.md          # Comprehensive runbook
├── logs/               # Log files
├── metrics/            # Metric snapshots
└── traces/             # Trace data
```

---

## Best Practices

### During Active Incidents

1. **Triage First**: Assess severity and consider immediate mitigation before investigation
2. **Document Everything**: Timeline, observations, actions - all in real-time
3. **Communicate**: Keep stakeholders informed of progress
4. **Mitigate, Then Investigate**: Stop the bleeding, then find root cause
5. **Blameless**: Focus on systems and processes, not people

### Investigation Approach

1. **Start Broad**: Gather all observability data first
2. **Correlate**: Find patterns across metrics, logs, traces
3. **Hypothesize**: Form testable hypotheses based on data
4. **Trace Code**: Validate hypotheses by tracing execution
5. **Five Whys**: Dig deep to find root cause, not symptoms

### Fix Implementation

1. **Test First**: Write tests that would have caught the issue
2. **Monitor**: Add metrics/alerts to detect recurrence
3. **Review**: Have fixes reviewed before deployment
4. **Document**: Capture learnings in runbook
5. **Share**: Spread knowledge across team

---

## Example Scenarios

### Example 1: Connection Pool Exhaustion

```bash
$ /debug.start API 500 errors. Connection pool exhausted. Started 15:45 UTC.
→ Analysis: Critical incident, recent deploy at 15:30

$ /debug.analyze
→ Metrics: Connection pool 100% at 15:45, query time 10x normal
→ Logs: "Connection not released" errors
→ Hypothesis: Connection leak in recent deploy

$ /debug.trace
→ Found: Error path not releasing connections
→ File: database.ts:145

$ /debug.diagnose
→ Root Cause: Missing finally block to release connections
→ Contributing: No connection pool monitoring

$ /debug.fix
→ Added: finally blocks, connection pool metrics, alerts

$ /debug.document
→ Created: "Connection Pool Exhaustion" runbook
```

### Example 2: Cascading Failure

```bash
$ /debug.start Service timeout cascade. A→B→C all failing.
→ Analysis: High severity, no recent deploys

$ /debug.analyze
→ Service A slow → B retry storm → C overload
→ Timeline: A(15:35) → B(15:38) → C(15:42)

$ /debug.trace
→ Service A external API timeout (30s)
→ Service B aggressive retries (no backoff)
→ Service C no circuit breaker

$ /debug.diagnose
→ Root Cause: No circuit breaker + aggressive retries
→ Contributing: External dependency without timeout

$ /debug.fix
→ Added: Circuit breakers, exponential backoff, timeouts

$ /debug.document
→ Created: "Cascading Failure Prevention" runbook
```

---

## Troubleshooting

### "No observability integration"

**Issue**: Observability systems not configured

**Solution**: Manually gather metrics/logs/traces and provide to workflow. Placeholders show what queries to run.

---

### "Cannot trace code path"

**Issue**: Agent cannot find relevant code

**Solution**: Provide more specific guidance (file names, function names) based on stack traces.

---

### "Hypothesis unclear"

**Issue**: Multiple possible root causes

**Solution**: Run `/debug.trace` multiple times for different hypotheses, or investigate top 2-3 in parallel.

---

## Customization

### Add Observability Integration

Edit `/debug.analyze` command to add actual API clients:

```bash
# Example: Add Prometheus integration
prometheus_query() {
  curl "http://prometheus:9090/api/v1/query?query=$1"
}
```

### Customize Runbook Template

Edit `/debug.document` command to match your team's runbook format.

### Add Custom Agents

Create specialized agents in `.claude/agents/` for domain-specific debugging.

---

## Comparison with Bugfix Workflow

| Aspect | Debugging Workflow | Bugfix Workflow |
|--------|-------------------|-----------------|
| **Scope** | Production incidents | Known bugs |
| **Depth** | Comprehensive investigation | Focused fix |
| **Observability** | Heavy emphasis | Minimal |
| **Output** | RCA + Runbook + Fix | Fix + PR |
| **Agents** | SRE + Explorer + TDD + Reviewer | Explorer + TDD + Reviewer |
| **Duration** | Hours to days | Minutes to hours |
| **Use When** | Investigating unknown issues | Fixing known issues |

**Use Debugging** for: Production incidents, performance issues, mysterious failures
**Use Bugfix** for: Reported bugs, minor issues, straightforward fixes

---

## Future Enhancements

Planned improvements:
- [ ] Full Prometheus/Grafana integration
- [ ] ELK Stack integration
- [ ] Jaeger/Tempo integration
- [ ] Automated incident detection and workflow trigger
- [ ] Slack/PagerDuty integration
- [ ] Automatic runbook testing
- [ ] ML-based anomaly detection
- [ ] Historical incident pattern matching

---

## Contributing

To enhance this workflow:
1. Test in real incident scenarios
2. Add observability integrations
3. Improve agent prompts based on results
4. Share successful runbooks
5. Document edge cases

---

## License

[Include appropriate license information]
