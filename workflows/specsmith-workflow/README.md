# Specsmith Workflow

Transform feature ideas into implementation-ready plans through structured interviews with multi-agent collaboration.

## Quick Start

### Standard Workflow
```bash
/spec.interview "your feature description"  # Gather requirements
/spec.create                                 # Generate plan
/spec.refine "your feedback"                # Improve plan (optional)
/spec.validate                              # Quality check
```

### Speedrun (Automated)
```bash
/spec.speedrun "your feature description"   # End-to-end automation
```

## What It Creates

- **interview-notes.md** - Requirements with MoSCoW prioritization
- **PLAN.md** - Implementation plan (platform-ready)
- **validation-report.md** - Quality scores (0-100)
- **speedrun-summary.md** - Metrics (speedrun only)

All in: `artifacts/specsmith/`

## Commands

| Command | Purpose |
|---------|---------|
| `/spec.interview` | Structured requirements gathering (5-7 questions) |
| `/spec.create` | Generate implementation plan from interview |
| `/spec.refine` | Improve plan based on feedback |
| `/spec.validate` | Multi-agent quality check (5 dimensions) |
| `/spec.speedrun` | Automated end-to-end (minimal interaction) |

## Agent Personas

- **Quinn (Architect)** - System design, integration, scalability
- **Maya (Engineer)** - Implementation, files, code patterns
- **Alex (QA)** - Testing strategy, quality gates
- **Casey (PM)** - Requirements, user needs, business value
- **Dana (Writer)** - Documentation clarity, structure

## Validation Scoring

- **90-100**: Excellent - Ready for platform
- **80-89**: Good - Minor refinements recommended
- **70-79**: Acceptable - Improvements needed
- **<70**: Needs work - Refine before implementation

## Key Features

- **Codebase-grounded** - Plans reference actual files (Glob/Grep/Read)
- **Constitution check** - 4 gates (codebase-grounded, realistic, iterative, ownership)
- **Platform integration** - Conforms to Ambient Code Platform template
- **Quality gate** - Speedrun auto-refines if score < 70
- **Single source of truth** - PLAN.md only (no duplicate files)

## When to Use

✅ Use when you need systematic planning for new features
✅ Use when you want multi-perspective design review
✅ Use speedrun for rapid POC planning

❌ Don't use for trivial changes or simple bug fixes

## License

MIT
