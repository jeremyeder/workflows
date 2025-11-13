---
description: Continue the workflow with detailed planning and implementation
displayName: Next Step
icon: ⏭️
---

## Command: Next Step

This command demonstrates how to chain workflow commands together and build on previous steps.

### User Input

```text
$ARGUMENTS
```

**Optional Arguments**: This command can work with or without additional input.

---

## Prerequisites Check

Before proceeding, verify that previous steps were completed:

1. **Check for initial analysis**: Look for `artifacts/outputs/initial-analysis.md`
   - If NOT found: Inform user to run `/template.start` first
   - If found: Read it and continue

2. **Verify user context**: Ensure there's enough information to proceed
   - If missing key details: Ask clarifying questions
   - If ready: Move forward with implementation

**Example Check:**
```bash
# Read the initial analysis to understand context
Read: artifacts/outputs/initial-analysis.md
```

---

## Execution Steps

### 1. Review Previous Work

Load context from previous command:

- Read the initial analysis document
- Understand what was already decided
- Identify any open questions or blockers
- Summarize the current state

**Tell the user:**
"Based on your initial request for [TOPIC], I'll now [NEXT ACTION]."

### 2. Perform the Core Work

This section is highly workflow-specific. Common patterns:

**Option A: Analysis/Research**
- Launch exploration agents (code-explorer, etc.)
- Gather information from codebase or external sources
- Synthesize findings into a report

**Option B: Planning/Design**
- Create architectural diagrams or plans
- Propose multiple implementation approaches
- Get user decision on preferred approach

**Option C: Implementation**
- Write code based on previous planning
- Create or modify files
- Run tests or validation

**Option D: Review/Validation**
- Check work against requirements
- Run quality checks or linters
- Identify issues to fix

### 3. Create Output Artifacts

Generate the next set of workflow outputs:

```bash
# Create appropriate directory structure
mkdir -p artifacts/outputs/detailed-plan
```

Use Write tool to create documents:
- Detailed plan or implementation
- Decision log (what was decided and why)
- Next steps or remaining tasks

**File locations**: `artifacts/outputs/detailed-plan/*.md`

### 4. Update Progress

If using a task tracking system:

```
TodoWrite:
  - Mark previous steps as completed
  - Add new tasks based on what needs to happen next
  - Keep the user informed of progress
```

### 5. Present Results & Next Steps

Provide a clear summary:

1. **What I did**: Brief overview of work completed
2. **What I found/created**: Key findings or outputs
3. **Decisions needed**: Any user input required
4. **What's next**: Recommended next action

**Example Output:**
```
✅ Detailed plan created!

I've analyzed [TOPIC] and created:
  - Architecture design: artifacts/outputs/detailed-plan/architecture.md
  - Implementation plan: artifacts/outputs/detailed-plan/plan.md

Key decisions:
  - [DECISION 1]: [REASONING]
  - [DECISION 2]: [REASONING]

Next steps:
  - Review the plans above
  - If you approve, I can proceed with implementation
  - If you have concerns, let me know and I'll revise

Ready to proceed?
```

---

## Command Chaining Patterns

### Pattern 1: Linear Progression
Commands follow a fixed sequence:
```
/workflow.start → /workflow.plan → /workflow.implement → /workflow.review
```

### Pattern 2: Conditional Branching
Next command depends on results:
```
/workflow.start → /workflow.analyze
              ↓
If complex: /workflow.design
If simple:  /workflow.implement
```

### Pattern 3: Iterative Loops
Commands can repeat:
```
/workflow.implement → /workflow.test
                    ↓
         If tests fail: /workflow.fix → back to test
         If tests pass: /workflow.complete
```

### Pattern 4: User Choice
Let user decide next step:
```
/workflow.analyze → Present options → User picks → Run chosen command
```

---

## Working with Agents

Commands often launch specialized agents to handle complex tasks:

### Example: Launch Code Explorer Agent
```
Task tool with subagent_type: code-explorer
Prompt: "Find all authentication-related code in the codebase.
         Trace through the login flow comprehensively.
         Return a list of 5-10 key files to read."
```

### Example: Launch Multiple Agents in Parallel
```
Launch 3 agents simultaneously:
  1. code-explorer: Find similar features
  2. code-architect: Design the architecture
  3. code-reviewer: Check for potential issues
```

### After Agents Complete
- Read the files they identified
- Synthesize their findings
- Present a cohesive summary to the user

---

## State Management

### Passing State Between Commands

**Option 1: File-based State**
- Each command writes outputs to `artifacts/`
- Next command reads previous outputs
- Simple and transparent

**Option 2: Conversation Context**
- Rely on conversation history
- Claude remembers previous interactions
- Works for simple workflows

**Option 3: Explicit State File**
- Create `artifacts/state.json` with current state
- Each command updates the state
- Useful for complex workflows with many paths

### Example State File
```json
{
  "workflow": "template",
  "currentStep": "planning",
  "completed": ["start", "analyze"],
  "pending": ["implement", "test", "review"],
  "userInput": {
    "requirement": "Build authentication system",
    "preferredApproach": "OAuth2"
  },
  "artifacts": {
    "analysis": "artifacts/outputs/initial-analysis.md",
    "plan": "artifacts/outputs/detailed-plan/plan.md"
  }
}
```

---

## Error Handling

### Common Error Scenarios

**1. Missing Prerequisites**
```
Error: Cannot find initial analysis document.
Solution: Please run `/template.start` first to initialize the workflow.
```

**2. Invalid State**
```
Error: The previous step was not completed successfully.
Solution: Please review artifacts/outputs/errors.log and fix issues before continuing.
```

**3. User Input Required**
```
Error: I need more information to proceed.
Solution: Please answer the following questions: [QUESTIONS]
```

### Graceful Degradation
- If optional data is missing, proceed with warnings
- If critical data is missing, stop and ask user
- Always provide clear guidance on how to recover

---

## Notes for Workflow Creators

**Customization Checklist:**
- [ ] Define what "previous work" this command depends on
- [ ] Implement appropriate prerequisite checks
- [ ] Customize the core work section for your workflow's purpose
- [ ] Update output artifact paths and names
- [ ] Define clear next steps for the user
- [ ] Add error handling for your specific failure modes
- [ ] Test the command in isolation and as part of the full workflow

**Best Practices:**
- Always check prerequisites before doing work
- Read previous artifacts to understand context
- Use TodoWrite to show progress on multi-step operations
- Launch agents for complex analysis or research tasks
- Create clear, well-organized output artifacts
- Provide actionable next steps
- Handle errors gracefully with helpful messages
- Test the complete workflow end-to-end

**Testing Your Command:**
1. Test with prerequisites met: Does it proceed correctly?
2. Test without prerequisites: Does it prompt appropriately?
3. Test state transitions: Does it update state correctly?
4. Test agent integration: Do agents return useful results?
5. Test error scenarios: Are errors handled gracefully?
6. Test the complete workflow: Does the flow make sense?
