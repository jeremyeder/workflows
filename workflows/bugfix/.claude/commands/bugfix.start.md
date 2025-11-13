---
description: Analyze a bug from description, GitHub issue, or Jira ticket
displayName: Analyze Bug
icon: 🔍
---

## Analyze Bug

Analyze a bug and prepare for automated fixing.

### User Input

```text
$ARGUMENTS
```

---

## Execution Steps

### 1. Parse Input

Determine the input type:

**GitHub Issue URL/Number:**
- Patterns: `https://github.com/owner/repo/issues/123`, `#123`, `GH-123`
- Extract: owner, repo, issue number
- Fetch issue details using `gh issue view`

**Jira Ticket:**
- Patterns: `PROJ-123`, `https://jira.*/browse/PROJ-123`
- Extract: ticket ID
- Note: Fetch details if Jira API is configured (placeholder for now)

**Plain Text Description:**
- Everything else is treated as a bug description
- Proceed with the description as-is

**Example:**
```bash
# For GitHub issue
gh issue view 123 --json title,body,labels --repo owner/repo

# For Jira (placeholder)
echo "Jira integration: TODO - fetch ticket details"
```

### 2. Create Bug Workspace

Set up a dedicated workspace for this bug:

```bash
# Generate bug ID (timestamp-based or issue number)
BUG_ID=$(date +%Y%m%d-%H%M%S)  # or use issue number if available

# Create directories
mkdir -p "artifacts/bugfixes/$BUG_ID"

# Initialize git branch
BRANCH_NAME="bugfix/$BUG_ID"
git checkout -b "$BRANCH_NAME"
```

### 3. Analyze the Bug

Use the **code-explorer** agent to understand the bug:

```
Task tool with subagent_type: code-explorer
Prompt: "Analyze this bug: [BUG_DESCRIPTION]

Find:
1. Where the bug is occurring (files, functions, lines)
2. What causes the bug (root cause)
3. Related code that might be affected
4. Similar issues in the codebase

Trace through the code comprehensively to understand the full context.
Return a list of 5-10 key files to examine."
```

After agent completes:
- Read all identified files
- Understand the bug's scope and impact
- Identify edge cases

### 4. Document Analysis

Create analysis document:

```markdown
# Bug Analysis: [BUG_ID]

## Bug Description
[Description from user input or issue]

## Source
- Type: [GitHub Issue / Jira Ticket / User Report]
- Reference: [URL or ID if applicable]

## Root Cause
[What causes the bug]

## Location
- Files: [list of affected files]
- Functions: [specific functions with issues]
- Lines: [approximate line numbers]

## Impact
- Severity: [Critical / High / Medium / Low]
- Affected Users: [description]
- Scope: [how widespread]

## Related Code
[Other parts of codebase that might be affected]

## Fix Strategy
[High-level approach to fixing]

## Edge Cases
[Cases to consider when implementing fix]
```

Save to: `artifacts/bugfixes/$BUG_ID/analysis.md`

### 5. Present Summary

Provide user with brief summary:

```
✅ Bug analysis complete!

Bug ID: [BUG_ID]
Branch: [BRANCH_NAME]
Root Cause: [One sentence summary]

Analysis saved to: artifacts/bugfixes/[BUG_ID]/analysis.md

Next step: Run `/bugfix.fix` to implement the fix automatically.
```

Use TodoWrite to track progress:
```
TodoWrite:
  [✅] Analyze bug
  [ ] Implement fix
  [ ] Verify with tests
  [ ] Create pull request
```

---

## Error Handling

**Invalid GitHub Issue:**
```
Error: Could not fetch GitHub issue.
Solutions:
  - Verify issue number and repository
  - Check gh CLI is authenticated: gh auth status
  - Provide bug description instead
```

**No Input Provided:**
```
Error: No bug information provided.

Usage:
  /bugfix.start {bug description}
  /bugfix.start https://github.com/owner/repo/issues/123
  /bugfix.start #123
  /bugfix.start PROJ-123
```

**Git Branch Already Exists:**
```
Warning: Branch bugfix/[ID] already exists.
Solutions:
  - Switch to existing branch: git checkout bugfix/[ID]
  - Delete and recreate: git branch -D bugfix/[ID]
  - Use different bug ID
```

---

## Examples

**Example 1: GitHub Issue**
```
User: /bugfix.start https://github.com/myorg/myapp/issues/456

Agent: Fetching GitHub issue #456...
✅ Bug analysis complete!

Bug ID: GH-456
Branch: bugfix/GH-456
Root Cause: Null pointer exception in user validation

Next step: Run `/bugfix.fix`
```

**Example 2: Jira Ticket**
```
User: /bugfix.start WEBAPP-789

Agent: Analyzing Jira ticket WEBAPP-789...
✅ Bug analysis complete!

Bug ID: WEBAPP-789
Branch: bugfix/WEBAPP-789
Root Cause: Race condition in async data loading

Next step: Run `/bugfix.fix`
```

**Example 3: Text Description**
```
User: /bugfix.start Login fails when username contains special characters

Agent: Analyzing bug from description...
✅ Bug analysis complete!

Bug ID: 20250113-143022
Branch: bugfix/20250113-143022
Root Cause: Input sanitization missing for usernames

Next step: Run `/bugfix.fix`
```
