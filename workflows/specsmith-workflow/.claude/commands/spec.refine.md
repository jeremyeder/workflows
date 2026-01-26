---
description: Iteratively improve plan based on feedback
displayName: spec.refine
icon: ✨
---

# spec.refine

Improve implementation plan through targeted agent collaboration.

## Prerequisites

Requires `PLAN.md` from `/spec.create`.

## Process

1. Parse refinement feedback from arguments
2. Read current PLAN.md
3. Invoke relevant agents based on scope:
   - Architecture changes → @quinn-architect
   - Implementation → @maya-engineer
   - Testing → @alex-qa
   - Requirements → @casey-pm
   - Documentation → @dana-tech-writer
4. Apply targeted updates to PLAN.md
5. Update status header (increment iteration count)

## Output

Updates `PLAN.md` with refinements. Changes tracked in git history.

## Next Steps

- Refine again: `/spec.refine "more feedback"`
- Validate: `/spec.validate`
