# Workflow Template

A comprehensive template demonstrating all available fields and patterns for creating ACP (Ambient Code Platform) workflows.

---

## 🎯 New to Workflows? Start Here!

**Never created a workflow before?** Check out these resources first:

1. **[hello-world workflow](../hello-world/)** - The absolute simplest example (~40 lines total)
2. **Read the sections below** - Conceptual overview → Visual guide → Tutorial
3. **Come back here** - When you need all the features and options

---

## What Are ACP Workflows?

Think of workflows as **recipe books for Claude Code**. They tell Claude:
- What role to play (assistant, debugger, code reviewer)
- What commands are available (`/start`, `/fix`, `/analyze`)
- How to organize outputs (where files go, what they're called)
- What to say when starting up (friendly greeting and instructions)

### Real-World Analogy

```
Cookbook (Workflow)           Recipe Book for Claude
─────────────────            ─────────────────────
📖 Title                  →  "name": "Fix Bugs"
📝 Description            →  "description": "Auto-fix bugs"
👨‍🍳 Chef Instructions      →  "systemPrompt": "You are a debugger..."
🎯 Recipes (Commands)     →  /bugfix.start, /bugfix.fix
📦 Serving Dishes         →  "results": Where outputs go
```

When you activate a workflow, Claude becomes that specialized assistant!

---

## How Workflows Work: Visual Guide

### The Big Picture

```
┌─────────────────────────────────────────────────────────────┐
│                     ACP Workflow System                      │
└─────────────────────────────────────────────────────────────┘

      User activates workflow
              ↓
┌─────────────────────────────┐
│  workflows/my-workflow/     │  ← Your workflow logic
│  ├── .ambient/              │
│  │   └── ambient.json       │  ← Configuration
│  └── .claude/               │
│      └── commands/          │
│          └── start.md       │  ← Slash commands
└─────────────────────────────┘
              ↓
      User runs /start
              ↓
┌─────────────────────────────┐
│  Claude executes command    │
│  - Reads user input         │
│  - Follows command steps    │
│  - Creates outputs          │
└─────────────────────────────┘
              ↓
┌─────────────────────────────┐
│  artifacts/                 │  ← Outputs (separate!)
│  └── outputs/               │
│      └── result.md          │
└─────────────────────────────┘
```

### File Organization

```
Repository Root
│
├── workflows/                    ← ALL workflow logic
│   ├── hello-world/             ← Simplest example
│   ├── template/                ← You are here
│   ├── bugfix/                  ← Real workflow example
│   └── my-workflow/             ← Your new workflow
│       ├── .ambient/
│       │   └── ambient.json     ← Config (name, prompts, results)
│       └── .claude/
│           ├── agents/          ← Optional AI personas
│           │   └── expert.md
│           └── commands/        ← Your slash commands
│               ├── start.md
│               └── next.md
│
└── artifacts/                    ← ALL outputs (gitignored)
    └── [workflow generates here]
```

**Key Insight**: Workflow code and outputs are **completely separate**!

### Command Execution Flow

```
User types: /myworkflow.start "build a login form"
                    ↓
┌───────────────────────────────────────────────────────────┐
│ 1. Claude reads: .claude/commands/myworkflow.start.md    │
└───────────────────────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────────────────────┐
│ 2. Command sees user input in $ARGUMENTS variable        │
│    $ARGUMENTS = "build a login form"                     │
└───────────────────────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────────────────────┐
│ 3. Command executes steps:                               │
│    - Validate input                                       │
│    - Do the work (analysis, coding, etc.)                │
│    - Create output files                                 │
│    - Tell user what's next                               │
└───────────────────────────────────────────────────────────┘
                    ↓
┌───────────────────────────────────────────────────────────┐
│ 4. Outputs appear in artifacts/                          │
│    User sees: "✅ Created plan at artifacts/plan.md"     │
└───────────────────────────────────────────────────────────┘
```

---

## Beginner Tutorial: Build Your First Workflow

Let's build a **"Coffee Order"** workflow step-by-step! ☕

### Step 1: Create the Structure

```bash
# From repo root
mkdir -p workflows/coffee-order/.ambient
mkdir -p workflows/coffee-order/.claude/commands
cd workflows/coffee-order
```

You now have:
```
workflows/coffee-order/
├── .ambient/          ← Will hold ambient.json
└── .claude/
    └── commands/      ← Will hold your commands
```

### Step 2: Create ambient.json

Create `.ambient/ambient.json`:

```json
{
  "name": "Coffee Order",
  "description": "Takes coffee orders and generates order summaries",
  "systemPrompt": "You are a friendly barista assistant. Use /order to take coffee orders.",
  "startupPrompt": "☕ Welcome to Coffee Order! Run `/order {drink}` to place an order. Example: `/order large latte`",
  "results": {
    "Order Summary": "artifacts/orders/*.md"
  }
}
```

**What each field does:**
- `name` - Shows in UI: "Coffee Order"
- `description` - Explains purpose
- `systemPrompt` - Tells Claude: "You're a barista, use /order command"
- `startupPrompt` - Greets user with example
- `results` - Tracks orders in artifacts/orders/

### Step 3: Create Your Command

Create `.claude/commands/order.md`:

```markdown
---
description: Take a coffee order and generate summary
---

## Take Coffee Order

### User Input

\`\`\`text
$ARGUMENTS
\`\`\`

---

## Steps

1. Parse the order from $ARGUMENTS
2. Create order summary
3. Save to file
4. Confirm with user

---

## Implementation

Parse the order:
- If $ARGUMENTS is empty, ask what they want
- Extract: size, drink type, modifications

Create directory:
\`\`\`bash
mkdir -p artifacts/orders
\`\`\`

Generate order summary:
\`\`\`markdown
# Coffee Order

**Drink**: [drink type]
**Size**: [size]
**Modifications**: [any special requests]

**Order Time**: [timestamp]
**Order ID**: [random number]

## Total
$[calculated price]

---
Thank you! Your order will be ready soon! ☕
\`\`\`

Save to: \`artifacts/orders/order-[id].md\`

Confirm:
\`\`\`
✅ Order received!

Your [size] [drink] is being prepared!

Order summary: artifacts/orders/order-[id].md

Total: $[price]
\`\`\`
```

### Step 4: Test It!

Activate your workflow and try:

```bash
/order large latte
/order small cappuccino with extra foam
/order
```

**Expected output:**
```
✅ Order received!

Your large latte is being prepared!

Order summary: artifacts/orders/order-12345.md

Total: $4.50
```

### Step 5: Add Another Command

Want to view all orders? Create `.claude/commands/history.md`:

```markdown
---
description: View all previous orders
---

## Order History

List all orders from \`artifacts/orders/*.md\`

Show:
- Order ID
- Drink
- Time
- Price

Format as a nice table.
```

Now users can run `/history` too!

---

## Understanding the Pieces

### What Makes a Minimal Workflow?

**Absolute minimum (2 files):**
```
workflows/my-workflow/
├── .ambient/ambient.json    ← Configuration (required)
└── .claude/commands/
    └── start.md             ← At least one command (required)
```

**Common workflow (3-5 files):**
```
workflows/my-workflow/
├── .ambient/ambient.json    ← Config
└── .claude/commands/
    ├── start.md             ← Initialize
    ├── next.md              ← Continue
    └── finish.md            ← Complete
```

**Advanced workflow (many files):**
```
workflows/my-workflow/
├── .ambient/ambient.json
├── .claude/
│   ├── agents/              ← Custom AI personas
│   │   ├── expert.md
│   │   └── reviewer.md
│   └── commands/
│       ├── start.md
│       ├── analyze.md
│       ├── design.md
│       └── implement.md
└── scripts/                 ← Optional helper scripts
    └── helpers.sh
```

### Visual: Command Structure

```
┌─────────────────────────────────────────┐
│ .claude/commands/mycommand.md           │
├─────────────────────────────────────────┤
│ ---                                     │
│ description: What this command does     │  ← Frontmatter
│ displayName: Short Name                 │    (YAML)
│ icon: 🚀                                │
│ ---                                     │
├─────────────────────────────────────────┤
│                                         │
│ ## Command Description                  │
│                                         │
│ ### User Input                          │
│ \`\`\`text                              │
│ $ARGUMENTS                              │  ← User's input
│ \`\`\`                                  │
│                                         │
├─────────────────────────────────────────┤
│                                         │
│ ## Execution Steps                      │  ← What to do
│                                         │
│ 1. Validate input                       │
│ 2. Do the work                          │
│ 3. Create outputs                       │
│ 4. Tell user what's next                │
│                                         │
└─────────────────────────────────────────┘
```

---

## Purpose

This template serves as a **reference implementation** showing:
- All available `ambient.json` configuration fields with inline documentation
- Example agent structure with complete frontmatter options
- Example command structures demonstrating various patterns
- Best practices for workflow design and implementation

Use this template as a starting point when creating new workflows for the vTeam/ACP platform.

---

## Directory Structure

```
workflows/template/
├── .ambient/
│   └── ambient.json              # Workflow configuration (REQUIRED)
├── .claude/
│   ├── agents/
│   │   └── example-agent.md      # Example agent persona (OPTIONAL)
│   └── commands/
│       ├── template.start.md     # Entry point command
│       └── template.next.md      # Follow-up command
└── README.md                     # This file
```

---

## Quick Start: Creating a New Workflow

### Step 1: Copy the Template

```bash
# From the repository root
cp -r workflows/template workflows/my-workflow
cd workflows/my-workflow
```

### Step 2: Customize ambient.json

Edit `.ambient/ambient.json`:

```json
{
  "name": "My Workflow Name",
  "description": "What this workflow does and when to use it",
  "systemPrompt": "You are a [ROLE] assistant. Use /myworkflow.start to begin.",
  "startupPrompt": "Greeting and instructions for the user",
  "results": {
    "Output Name": "artifacts/path/to/outputs/**/*.md"
  }
}
```

**Key fields to update:**
- `name`: Display name in the UI
- `description`: Brief explanation of the workflow
- `systemPrompt`: Instructions for Claude (methodology, available commands, behavior)
- `startupPrompt`: What Claude says when starting the workflow
- `results`: Map output names to file path patterns (relative to `artifacts/`)

### Step 3: Create Your Commands

Rename and customize commands in `.claude/commands/`:

```bash
mv .claude/commands/template.start.md .claude/commands/myworkflow.start.md
mv .claude/commands/template.next.md .claude/commands/myworkflow.next.md
```

Edit each command file:
1. Update the frontmatter (description, displayName, icon)
2. Customize the execution steps for your workflow logic
3. Update input validation and output paths
4. Modify the "next steps" guidance

### Step 4: Customize or Remove Agents (Optional)

If you need specialized agent personas:

```bash
mv .claude/agents/example-agent.md .claude/agents/my-agent.md
```

Edit the agent file:
1. Update frontmatter (name, description, tools)
2. Customize personality and communication style
3. Define domain-specific skills
4. Set working principles

If you don't need custom agents, delete the `.claude/agents/` directory.

### Step 5: Test Your Workflow

1. Ensure your workflow is in the `workflows/` directory
2. Activate the workflow in your ACP session
3. Test the startup prompt
4. Test each command with various inputs
5. Verify outputs are created in the correct locations
6. Check error handling and edge cases

---

## Configuration Reference

### ambient.json Schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | ✅ | Display name shown in UI |
| `description` | string | ✅ | Brief description of the workflow |
| `systemPrompt` | string | ✅ | Instructions for Claude (context, commands, behavior) |
| `startupPrompt` | string | ✅ | What Claude says/does when starting the workflow |
| `results` | object | ✅ | Map of output names to file path patterns (supports globs) |

### Command Frontmatter

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `description` | string | ✅ | Brief description of what this command does |
| `displayName` | string | ❌ | Short display name (2-4 words) |
| `icon` | string | ❌ | Emoji icon shown in UI |

### Agent Frontmatter

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | ✅ | Agent name and role |
| `description` | string | ✅ | What the agent does and when to use it |
| `tools` | string | ❌ | Comma-separated list of allowed tools |

**Available Tools**: Read, Write, Edit, Bash, Glob, Grep, WebSearch, WebFetch, TodoWrite, Task, NotebookRead, NotebookEdit

---

## Design Patterns

### Pattern 1: Linear Workflow

Commands follow a fixed sequence:

```
Commands:
  /workflow.start   → Initialize and gather requirements
  /workflow.plan    → Create implementation plan
  /workflow.execute → Implement the plan
  /workflow.review  → Validate and review

Each command checks that previous steps are complete.
```

### Pattern 2: Branching Workflow

Commands adapt based on results:

```
Commands:
  /workflow.start   → Analyze the request
                    ↓
  /workflow.simple  → For simple cases (quick implementation)
  /workflow.complex → For complex cases (detailed planning first)
```

### Pattern 3: Iterative Workflow

Commands loop until success:

```
Commands:
  /workflow.implement → Create the solution
  /workflow.test      → Run tests
                      ↓
  If tests fail:      /workflow.fix → back to /workflow.test
  If tests pass:      /workflow.complete
```

### Pattern 4: Hub-and-Spoke Workflow

Central command delegates to specialists:

```
Commands:
  /workflow.start   → Main orchestrator
                    ↓
  Launches agents or sub-commands based on task type:
    - /workflow.analyze
    - /workflow.design
    - /workflow.implement
    - /workflow.document
```

---

## Best Practices

### Command Design

1. **Single Responsibility**: Each command should do one thing well
2. **Clear Prerequisites**: Check and communicate what's needed before running
3. **Validation First**: Validate inputs before doing work
4. **Idempotent**: Running the same command twice should be safe
5. **Informative Output**: Always tell the user what happened and what's next

### State Management

1. **File-Based State**: Use artifacts for passing data between commands
2. **Clear Naming**: Use descriptive file names and directory structure
3. **Separation**: Keep workflow logic (in `workflows/`) separate from outputs (in `artifacts/`)
4. **Versioning**: Consider timestamping or versioning artifacts for complex workflows

### Error Handling

1. **Graceful Failures**: Provide clear error messages and recovery steps
2. **Prerequisite Checks**: Fail fast if prerequisites aren't met
3. **User Guidance**: Always tell the user how to fix the problem
4. **Partial Success**: Save progress even if the full command fails

### Agent Integration

1. **Specific Tasks**: Use agents for well-defined complex tasks
2. **Parallel Execution**: Launch multiple agents in parallel when possible
3. **Read Results**: Always read the files agents identify
4. **Synthesize**: Combine agent findings into a cohesive summary

### User Experience

1. **Clear Instructions**: Tell users what to do next
2. **Progress Tracking**: Use TodoWrite to show progress on multi-step operations
3. **Confirmation**: Ask before making significant changes
4. **Feedback**: Provide feedback at each step
5. **Examples**: Include example usage in command documentation

---

## File Organization

### Where to Put Things

```
workflows/my-workflow/          # Workflow logic and templates
├── .ambient/
│   └── ambient.json           # Configuration only
├── .claude/
│   ├── agents/               # AI personas (optional)
│   └── commands/             # Slash commands (required)
└── scripts/                  # Helper scripts (optional)

artifacts/                     # Outputs (separate from workflow!)
├── outputs/                  # Generated documents
├── plans/                    # Implementation plans
└── state.json               # Workflow state (optional)
```

### Path Conventions

**In ambient.json:**
- Paths in `results` are relative to `artifacts/`
- Use glob patterns for matching multiple files: `**/*.md`

**In commands:**
- Reference artifacts with full path: `artifacts/outputs/file.md`
- Create directories before writing: `mkdir -p artifacts/outputs`

**In agents:**
- Agents inherit the working directory from the workflow
- Use absolute paths or paths relative to workflow directory

---

## Common Workflows Examples

### Example 1: Analysis Workflow

**Purpose**: Analyze code or documents and produce reports

```
Commands:
  /analyze.start {target}   → Initialize and validate target
  /analyze.scan            → Scan and gather data
  /analyze.report          → Generate analysis report

Results:
  "Analysis Report": "artifacts/reports/**/*.md"
```

### Example 2: Code Generation Workflow

**Purpose**: Generate code based on specifications

```
Commands:
  /codegen.spec {description}  → Capture requirements
  /codegen.design             → Design architecture
  /codegen.generate           → Generate code
  /codegen.test               → Create and run tests

Results:
  "Specification": "artifacts/specs/*.md"
  "Generated Code": "artifacts/code/**/*"
  "Tests": "artifacts/tests/**/*"
```

### Example 3: Documentation Workflow

**Purpose**: Create comprehensive documentation

```
Commands:
  /docs.init {topic}      → Initialize documentation project
  /docs.outline          → Create documentation outline
  /docs.write            → Write documentation sections
  /docs.review           → Review and finalize

Results:
  "Documentation": "artifacts/docs/**/*.md"
```

---

## Troubleshooting

### Issue: Command not found

**Problem**: `/myworkflow.start` returns "command not found"

**Solutions**:
1. Ensure file is named correctly: `.claude/commands/myworkflow.start.md`
2. Check that the workflow directory is in the `workflows/` folder
3. Verify the workflow has been activated in the ACP session

### Issue: Outputs not appearing in results

**Problem**: Files created but not showing in workflow results

**Solutions**:
1. Verify paths in `ambient.json` `results` field match actual output locations
2. Ensure paths are relative to `artifacts/` directory
3. Check glob patterns are correct (e.g., `**/*.md` for all markdown files)

### Issue: Agent not responding correctly

**Problem**: Agent behavior doesn't match persona

**Solutions**:
1. Check agent frontmatter is valid YAML
2. Verify `tools` field lists correct tool names
3. Ensure agent persona instructions are clear and specific
4. Test agent in isolation before integrating into workflow

### Issue: State not persisting between commands

**Problem**: Commands can't see data from previous commands

**Solutions**:
1. Ensure previous command wrote outputs to `artifacts/`
2. Verify next command is reading from correct file paths
3. Check file paths are absolute or correctly relative
4. Consider creating explicit state file for complex workflows

---

## Migration Guide

### Converting Existing Workflows

If you have an existing workflow to migrate:

1. **Extract Configuration**: Move workflow config to `ambient.json`
2. **Split Commands**: Separate monolithic scripts into individual commands
3. **Define Agents**: Extract specialized behaviors into agent personas
4. **Update Paths**: Ensure all paths follow the artifacts/ convention
5. **Test Thoroughly**: Verify all commands work in the new structure

### Upgrading This Template

When new workflow features become available:

1. Check the `_schema_version` in `ambient.json`
2. Review new fields or capabilities in documentation
3. Update your workflow's `ambient.json` to use new features
4. Test to ensure backward compatibility

---

## Additional Resources

- **ACP Documentation**: [Link to main ACP docs]
- **Example Workflows**: See other workflows in `workflows/` directory
- **Workflow Registry**: [Link to workflow registry if available]

---

## Contributing

If you create a useful workflow pattern or improvement:

1. Document it clearly
2. Add examples
3. Submit for inclusion in the template or as a separate example workflow

---

## License

[Include appropriate license information]
