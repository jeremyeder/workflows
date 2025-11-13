---
# REQUIRED FIELDS:

# Brief description of what this command does
description: Initialize the workflow and gather requirements from user input

# OPTIONAL FIELDS:

# Short display name shown in UI (keep it concise, 2-4 words)
displayName: Start Workflow

# Emoji icon shown next to command in UI
icon: 🚀
---

## Command: Start Workflow

This command demonstrates the structure and available features of ACP workflow commands.

### User Input

```text
$ARGUMENTS
```

**Available Variables:**
- `$ARGUMENTS` - Everything the user typed after the command name

**Input Handling:**
You **MUST** consider the user input before proceeding. If `$ARGUMENTS` is empty, prompt the user for required information.

---

## Command Overview

**Purpose**: Initialize the workflow, validate inputs, and set up the working environment

**Prerequisites**: None (this is the entry point command)

**Outputs**:
- Initial analysis document
- Setup confirmation
- Next steps guidance

---

## Execution Steps

### 1. Validate Input

First, check if the user provided required information:

```
If $ARGUMENTS is empty:
  - Explain what information is needed
  - Provide example input format
  - Ask user to provide the input
  - STOP (don't proceed until input is provided)

If $ARGUMENTS is provided:
  - Acknowledge the input
  - Summarize what you understood
  - Proceed to step 2
```

### 2. Analyze Requirements

Break down the user's request:

- **What** are they trying to accomplish?
- **Why** is this needed? (understand the goal)
- **How** should it work? (behavior and constraints)
- **When** should it be completed? (timeline/urgency)
- **Who** will use it? (audience/users)

Document your analysis in a clear summary.

### 3. Ask Clarifying Questions (If Needed)

Use the `AskUserQuestion` tool if any of these are unclear:

- Scope boundaries (what's included/excluded)
- Success criteria (how to measure completion)
- Constraints (technical, timeline, resources)
- Preferences (approach, tools, patterns)
- Dependencies (related systems, features)

**Example Question Pattern:**
```
AskUserQuestion:
  - Question: "How should the system handle [EDGE CASE]?"
    Options:
      a) [APPROACH A] - [TRADEOFFS]
      b) [APPROACH B] - [TRADEOFFS]
      c) [APPROACH C] - [TRADEOFFS]
```

### 4. Create Initial Artifacts

Generate the first workflow output:

```bash
# Create output directory if it doesn't exist
mkdir -p artifacts/outputs
```

Create initial document using Write tool:
- Summarize user request
- Document requirements
- List assumptions
- Identify next steps

**File location**: `artifacts/outputs/initial-analysis.md`

### 5. Present Summary & Next Steps

Provide the user with:

1. **What I understood**: Brief summary of their request
2. **What I created**: Link to the analysis document
3. **What's next**: Recommended next command to run
4. **Questions**: Any remaining clarifications needed

**Example Output:**
```
✅ Workflow initialized!

I understood that you want to [SUMMARY].

I've created an initial analysis document at:
  artifacts/outputs/initial-analysis.md

Next steps:
  Run `/template.next` to continue with [NEXT PHASE]

Questions? Just ask!
```

---

## Command Examples

### Example 1: With Arguments
```
User: /template.start Build a user authentication system

Agent: I'll help you build a user authentication system.

[Analysis and document creation...]

✅ Created initial analysis at artifacts/outputs/initial-analysis.md
Run `/template.next` to design the architecture.
```

### Example 2: Without Arguments
```
User: /template.start

Agent: I need more information to get started.

Please provide a description of what you want to accomplish.

Example: /template.start Build a user authentication system

What would you like to work on?
```

---

## Notes for Workflow Creators

**Customization Checklist:**
- [ ] Update the description, displayName, and icon
- [ ] Modify the validation logic for your workflow's input requirements
- [ ] Customize the analysis steps for your specific use case
- [ ] Change output file paths and names as needed
- [ ] Update the "next command" recommendation
- [ ] Add workflow-specific questions or checks
- [ ] Include any setup scripts or initialization tasks

**Best Practices:**
- Always validate input before proceeding
- Use TodoWrite to track multi-step operations
- Ask specific, concrete questions (avoid open-ended)
- Create artifacts in the `artifacts/` directory (NOT in `workflows/`)
- Reference next commands to guide users through the workflow
- Handle errors gracefully and provide clear error messages

**Testing Your Command:**
1. Test with valid input: Does it proceed correctly?
2. Test with empty input: Does it prompt appropriately?
3. Test with invalid input: Does it handle errors well?
4. Test the output artifacts: Are they created in the right location?
5. Test the user experience: Is the flow clear and intuitive?
