---
description: Gather requirements through structured interview
displayName: spec.interview
icon: 🎤
---

# spec.interview

Conduct structured conversation to gather requirements before planning.

## Process

1. Parse feature description from arguments
2. Discover codebase context (Glob/Grep/Read)
3. Invoke @quinn-architect and @casey-pm for analysis
4. Ask 5-7 clarifying questions (What/Who/Why/Where/How/When/Dependencies)
5. Generate `artifacts/specsmith/interview-notes.md` with MoSCoW prioritization

## Output

Creates `interview-notes.md` with:
- Feature Overview
- Requirements (Must/Should/Could/Won't Have)
- Technical Context
- Success Criteria

## Next Steps

Run `/spec.create` to generate implementation plan.
