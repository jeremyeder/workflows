---
description: Multi-perspective quality assessment
displayName: spec.validate
icon: ✅
---

# spec.validate

Comprehensive quality check across 5 dimensions.

## Prerequisites

Requires `PLAN.md` from `/spec.create`.

## Process

1. Invoke all 5 agents for multi-perspective review:
   - Architecture (20pts) - @quinn-architect
   - Implementation (20pts) - @maya-engineer
   - Testing (20pts) - @alex-qa
   - Requirements (20pts) - @casey-pm
   - Documentation (20pts) - @dana-tech-writer
2. Check constitution gates (4 checks)
3. Assess platform integration readiness
4. Generate validation-report.md with scores and findings
5. Update PLAN.md status header with quality score

## Output

Creates `validation-report.md` with:
- Overall score (0-100)
- Dimension breakdown
- Critical issues / warnings / recommendations

## Scoring

- 90-100: Excellent - Ready for platform
- 80-89: Good - Minor refinements recommended
- 70-79: Acceptable - Improvements needed
- <70: Needs work - Refine before implementation

## Next Steps

- If score ≥ 80: Hand PLAN.md to platform
- If score < 80: `/spec.refine` to address issues
