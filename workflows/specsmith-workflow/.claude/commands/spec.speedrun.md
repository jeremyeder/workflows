---
description: Automated end-to-end workflow
displayName: spec.speedrun
icon: ⚡
---

# spec.speedrun

Execute entire workflow automatically with minimal interaction.

## Process

1. Auto-interview: Ask 2-3 critical questions, detect tech stack, make codebase-driven assumptions
2. Auto-create: Invoke all agents in parallel, generate PLAN.md
3. Auto-validate: Multi-agent quality check
4. Quality gate: If score < 70, auto-refine TOP 3 issues and re-validate
5. Generate speedrun-summary.md with metrics and decisions

## Output

Creates all 4 artifacts:
- interview-notes.md
- PLAN.md
- validation-report.md
- speedrun-summary.md

## Use Cases

- Rapid POC generation
- Time-constrained scenarios
- Experienced users who trust codebase-driven assumptions

## Quality Gate

- If score < 70 after initial validation: Auto-refine targeted issues
- If still < 70 after 1 refinement: Stop, recommend manual `/spec.refine`
- If score ≥ 70: Success
