---
description: Generate implementation plan from interview notes
displayName: spec.create
icon: 📋
---

# spec.create

Transform interview notes into platform-ready implementation plan.

## Prerequisites

Requires `interview-notes.md` from `/spec.interview`.

## Process

1. Validate prerequisites (interview-notes.md exists)
2. Deep codebase analysis (Glob/Grep/Read for files, patterns, tests)
3. Invoke @quinn-architect, @maya-engineer, @alex-qa for multi-perspective design
4. Run constitution check (codebase-grounded, realistic, iterative, ownership clear)
5. Generate `artifacts/specsmith/PLAN.md` with platform template sections

## Output

Creates `PLAN.md` with:
- Summary + Technical Context
- Constitution Check
- Project Structure
- Phase 0: Research
- Phase 1: Data Model
- Phase 2: Task Breakdown

## Next Steps

- To refine: `/spec.refine "feedback"`
- To validate: `/spec.validate`
