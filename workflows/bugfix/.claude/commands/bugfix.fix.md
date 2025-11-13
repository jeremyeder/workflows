---
description: Implement the bug fix using TDD approach
displayName: Implement Fix
icon: 🔧
---

## Implement Bug Fix

Automatically implement the bug fix using test-driven development.

---

## Execution Steps

### 1. Load Bug Analysis

Read the analysis from the previous step:

```bash
# Find the most recent bug analysis
LATEST_BUG=$(ls -t artifacts/bugfixes/*/analysis.md | head -1)
BUG_DIR=$(dirname "$LATEST_BUG")
```

If no analysis found:
```
Error: No bug analysis found.
Please run `/bugfix.start` first to analyze the bug.
```

Read the analysis to understand:
- Root cause
- Affected files
- Fix strategy
- Edge cases

### 2. Implement Fix Using TDD

Use the **tdd-developer** agent for test-first development:

```
Task tool with subagent_type: tdd-developer
Prompt: "Implement a fix for this bug using TDD approach:

Bug Analysis: [Summary from analysis.md]

Follow the red-green-refactor cycle:
1. Write failing tests that demonstrate the bug
2. Implement the minimal fix to make tests pass
3. Refactor for clarity and maintainability

Files to modify: [List from analysis]
Fix strategy: [Strategy from analysis]
Edge cases to cover: [Cases from analysis]

Return a summary of:
- Tests created
- Code changes made
- Any additional files modified"
```

### 3. Verify the Fix

After implementation, run a quick sanity check:

```bash
# If there's a test command in package.json/pytest/etc
if [ -f "package.json" ]; then
  npm test 2>&1 | head -50
elif [ -f "pytest.ini" ] || [ -f "pyproject.toml" ]; then
  pytest -v 2>&1 | head -50
elif [ -f "Makefile" ] && grep -q "^test:" Makefile; then
  make test 2>&1 | head -50
fi
```

### 4. Document Implementation

Create implementation document:

```markdown
# Fix Implementation: [BUG_ID]

## Summary
[One paragraph describing what was fixed]

## Changes Made

### Tests Added
[List of test files and test cases]

### Code Changes
[List of files modified with brief description of changes]

### Files Modified
- `path/to/file1.js` - [description]
- `path/to/file2.py` - [description]

## Test Results
[Quick summary of test run]

## Verification
- [✅] Tests pass
- [✅] Bug is fixed
- [✅] No regressions introduced
- [✅] Edge cases handled

## Notes
[Any important implementation notes or decisions made]
```

Save to: `artifacts/bugfixes/$BUG_ID/implementation.md`

### 5. Update Progress

Update the todo list:

```
TodoWrite:
  [✅] Analyze bug
  [✅] Implement fix
  [ ] Verify with tests
  [ ] Create pull request
```

Present summary to user:

```
✅ Fix implemented!

Changed files:
  - [file1]: [description]
  - [file2]: [description]

Tests added:
  - [test1]: [description]
  - [test2]: [description]

Quick test run: [PASS/FAIL]

Implementation saved to: artifacts/bugfixes/[BUG_ID]/implementation.md

Next step: Run `/bugfix.verify` for comprehensive testing.
```

---

## Error Handling

**Tests Fail After Implementation:**
```
⚠️  Warning: Tests are failing after fix implementation.

Failed tests:
  [List of failing tests]

I'll analyze the failures and attempt another fix.
[Re-run tdd-developer agent with additional context]
```

**Cannot Find Files to Fix:**
```
Error: Could not locate files mentioned in analysis.

Analysis said to modify: [files]
But these files don't exist.

Please verify the analysis or update file paths manually.
```

**Merge Conflicts:**
```
Error: Current branch has conflicts with main.

Please resolve conflicts:
  git pull origin main
  # Fix conflicts
  git add .

Then run `/bugfix.fix` again.
```

---

## Examples

**Example 1: Simple Bug Fix**
```
User: /bugfix.fix

Agent: Reading bug analysis...
Agent: Implementing fix using TDD...

✅ Fix implemented!

Changed files:
  - src/auth/validator.js: Added input sanitization
  - tests/auth/validator.test.js: Added tests for special characters

Tests added:
  - "validates username with special chars"
  - "rejects invalid special char combinations"

Quick test run: ✅ All tests pass (12/12)

Next step: Run `/bugfix.verify`
```

**Example 2: Complex Fix with Refactoring**
```
User: /bugfix.fix

Agent: Reading bug analysis...
Agent: Implementing fix using TDD...

✅ Fix implemented!

Changed files:
  - src/data/loader.js: Fixed race condition with mutex
  - src/data/cache.js: Refactored cache invalidation
  - tests/data/loader.test.js: Added concurrency tests
  - tests/data/cache.test.js: Added cache consistency tests

Tests added:
  - "handles concurrent data loads safely"
  - "prevents race condition in cache updates"
  - "maintains cache consistency under load"

Quick test run: ✅ All tests pass (28/28)

Next step: Run `/bugfix.verify`
```

---

## Implementation Notes

**TDD Workflow:**
1. Agent writes tests that fail (demonstrating the bug)
2. Agent implements minimal code to pass tests
3. Agent refactors for quality
4. Repeat if needed for edge cases

**Automatic Decisions:**
- Choice of test framework (based on project conventions)
- Test placement (follows existing test structure)
- Code style (matches project formatting)
- Import organization (follows project patterns)

**Manual Review Triggers:**
- If fix requires architectural changes (agent asks first)
- If fix affects public APIs (agent confirms approach)
- If tests can't be made to pass (agent reports and asks for help)
