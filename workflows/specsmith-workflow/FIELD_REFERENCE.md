# Field Reference

Quick reference for Specsmith workflow artifacts and scores.

## Artifacts

### interview-notes.md
- Feature Overview
- Requirements (MoSCoW: Must/Should/Could/Won't Have)
- Technical Context
- Success Criteria

### PLAN.md (Single Source of Truth)
**Status Header**:
```markdown
**Status**: [Phase] ([Iteration])
**Last Updated**: [Timestamp]
**Quality Score**: [Score]/100
```

**Required Sections** (Platform Template):
- Summary
- Technical Context (8 fields: language, dependencies, storage, testing, platform, performance, constraints, scale)
- Constitution Check (4 gates)
- Project Structure
- Phase 0: Research
- Phase 1: Data Model
- Phase 2: Task Breakdown

### validation-report.md
- Overall Score (0-100)
- 5 Dimension Scores (20 points each)
- Critical Issues / Warnings / Recommendations
- Agent Findings

### speedrun-summary.md
- Execution Time
- Key Decisions
- Assumptions Made
- Quality Metrics

## Quality Scores

| Range | Grade | Action |
|-------|-------|--------|
| 90-100 | Excellent | Implement immediately |
| 80-89 | Good | Implement or refine minors |
| 70-79 | Acceptable | Refine before implementation |
| <70 | Needs Work | Fix critical issues |

### Validation Dimensions (20 points each)

1. **Architecture** (@quinn-architect) - System design, integration, scalability
2. **Implementation** (@maya-engineer) - File paths, feasibility, dependencies
3. **Testing** (@alex-qa) - Strategy, coverage, quality gates
4. **Requirements** (@casey-pm) - User needs, business value, scope
5. **Documentation** (@dana-tech-writer) - Clarity, completeness, structure

## Constitution Check (4 Gates)

- ✅ **Codebase-Grounded** - References actual files with line numbers
- ✅ **Realistic Timeline** - Achievable phases and estimates
- ✅ **Iterative Phases** - Incremental delivery (Phase 0 → 1 → 2)
- ✅ **Ownership Clear** - Responsibilities defined

## Workflow Paths

### Standard
```
/spec.interview → /spec.create → /spec.refine (optional) → /spec.validate
```

### Speedrun
```
/spec.speedrun → [auto-interview, auto-create, auto-validate, auto-refine if needed]
```

## Agent Invocation

Commands automatically invoke relevant agents:

| Command | Agents Invoked |
|---------|---------------|
| interview | @quinn-architect, @casey-pm |
| create | @quinn-architect, @maya-engineer, @alex-qa |
| refine | Selected based on refinement type |
| validate | All 5 agents |
| speedrun | All 5 agents (parallel) |

## Progress Indicators

```markdown
Phase 1: Requirements Gathering ✅ COMPLETE
Phase 2: Plan Generation ✅ COMPLETE
Phase 3: Iterative Refinement 🔄 2 iterations
Phase 4: Quality Validation ⏳ PENDING
```

## Common Patterns

### MoSCoW Prioritization
- **Must Have** - Critical for MVP
- **Should Have** - Important but not critical
- **Could Have** - Nice to have
- **Won't Have** - Out of scope

### Codebase Discovery
```bash
Glob: "src/**/*.{ts,tsx,js,jsx}"  # Find files
Grep: pattern:"auth" output_mode:"files_with_matches"  # Search patterns
Read: "src/lib/auth.ts"  # Examine files
```
