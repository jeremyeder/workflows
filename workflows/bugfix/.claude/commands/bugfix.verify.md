---
description: Run comprehensive tests and quality checks
displayName: Verify Fix
icon: ✅
---

## Verify Bug Fix

Run comprehensive testing and quality checks before creating a PR.

---

## Execution Steps

### 1. Load Implementation Details

Read the implementation document:

```bash
# Find the most recent implementation
LATEST_IMPL=$(ls -t artifacts/bugfixes/*/implementation.md | head -1)
BUG_DIR=$(dirname "$LATEST_IMPL")
```

If no implementation found:
```
Error: No implementation found.
Please run `/bugfix.fix` first to implement the fix.
```

### 2. Run Comprehensive Test Suite

Execute all relevant tests:

```bash
# Detect and run test framework
if [ -f "package.json" ] && grep -q "\"test\":" package.json; then
  echo "Running npm tests..."
  npm test
elif [ -f "pytest.ini" ] || [ -f "pyproject.toml" ]; then
  echo "Running pytest..."
  pytest -v --cov
elif [ -f "Makefile" ] && grep -q "^test:" Makefile; then
  echo "Running make test..."
  make test
elif [ -f "go.mod" ]; then
  echo "Running go tests..."
  go test ./...
else
  echo "No test framework detected"
fi
```

Capture results:
- Total tests run
- Tests passed
- Tests failed
- Code coverage (if available)

### 3. Code Quality Review

Use the **code-reviewer** agent to review the fix:

```
Task tool with subagent_type: code-reviewer
Prompt: "Review this bug fix for quality and correctness:

Bug: [Summary from analysis]
Implementation: [Summary from implementation.md]
Changed files: [List of modified files]

Check for:
1. Correctness: Does the fix actually solve the bug?
2. Test Coverage: Are all edge cases tested?
3. Code Quality: Is the code clean, readable, maintainable?
4. Security: Any security implications?
5. Performance: Any performance impact?
6. Breaking Changes: Does it break existing functionality?

Provide a focused review with only high-priority issues."
```

### 4. Run Linters and Formatters

Ensure code meets project standards:

```bash
# JavaScript/TypeScript
if [ -f "package.json" ]; then
  if grep -q "\"lint\":" package.json; then
    npm run lint
  fi
  if grep -q "\"format\":" package.json; then
    npm run format --check || npm run format
  fi
fi

# Python
if [ -f "pyproject.toml" ] || [ -f "setup.py" ]; then
  if command -v black &> /dev/null; then
    black . --check || black .
  fi
  if command -v flake8 &> /dev/null; then
    flake8 .
  fi
  if command -v isort &> /dev/null; then
    isort . --check || isort .
  fi
fi

# Go
if [ -f "go.mod" ]; then
  go fmt ./...
  if command -v golint &> /dev/null; then
    golint ./...
  fi
fi
```

### 5. Build/Compile Check

Verify the project builds successfully:

```bash
# Node.js
if [ -f "package.json" ] && grep -q "\"build\":" package.json; then
  npm run build
fi

# Go
if [ -f "go.mod" ]; then
  go build ./...
fi

# Rust
if [ -f "Cargo.toml" ]; then
  cargo build
fi

# Java/Maven
if [ -f "pom.xml" ]; then
  mvn clean compile
fi
```

### 6. Create Test Results Document

Document all verification results:

```markdown
# Test Results: [BUG_ID]

## Test Execution

### Unit Tests
- **Total**: [number]
- **Passed**: [number]
- **Failed**: [number]
- **Coverage**: [percentage]%

### Test Output
```
[Relevant test output]
```

## Code Quality Review

### Correctness
[✅/⚠️] Fix solves the bug: [details]

### Test Coverage
[✅/⚠️] Edge cases covered: [details]

### Code Quality
[✅/⚠️] Code is clean and maintainable: [details]

### Security
[✅/⚠️] No security concerns: [details]

### Performance
[✅/⚠️] No performance regression: [details]

### Breaking Changes
[✅/⚠️] No breaking changes: [details]

## Linting & Formatting

- **Linter**: [PASS/FAIL] [details]
- **Formatter**: [PASS/FAIL] [details]
- **Type Check**: [PASS/FAIL] [details if applicable]

## Build Verification

- **Build Status**: [SUCCESS/FAIL]
- **Build Output**: [relevant details]

## Summary

**Overall Status**: [✅ READY / ⚠️ NEEDS ATTENTION / ❌ FAILED]

**Issues Found**: [number]
- [Critical issues if any]
- [Important warnings if any]

**Recommendation**:
[✅] Ready to submit PR
[⚠️] Fix [issues] before submitting
[❌] Fix requires rework
```

Save to: `artifacts/bugfixes/$BUG_ID/test-results.md`

### 7. Handle Verification Results

**If all checks pass:**
```
TodoWrite:
  [✅] Analyze bug
  [✅] Implement fix
  [✅] Verify with tests
  [ ] Create pull request
```

Present success message:
```
✅ All verification checks passed!

Test Results:
  ✅ All tests pass ([number]/[number])
  ✅ Code quality review: No critical issues
  ✅ Linters pass
  ✅ Build successful

Coverage: [percentage]%

Results saved to: artifacts/bugfixes/[BUG_ID]/test-results.md

Next step: Run `/bugfix.submit` to create a PR automatically.
```

**If checks fail:**
```
⚠️  Verification found issues:

Critical:
  - [issue 1]
  - [issue 2]

Warnings:
  - [warning 1]

I can attempt to fix these automatically. Continue? [Yes/No]
```

If user says yes, run `/bugfix.fix` again with additional context.
If user says no, wait for manual intervention.

---

## Error Handling

**Tests Fail:**
```
❌ Tests failed ([passed]/[total])

Failed tests:
  - [test 1]: [error]
  - [test 2]: [error]

Analysis: [What might be wrong]

Options:
  1. Run `/bugfix.fix` to attempt another fix
  2. Review test failures manually
  3. Skip failed tests (not recommended)
```

**Linting Errors:**
```
⚠️  Linting errors found:

[Linter output]

Attempting automatic fix...
[Run formatters]

✅ Linting errors fixed automatically
```

**Build Fails:**
```
❌ Build failed

Error:
[Build error output]

This usually indicates:
  - Syntax errors in the fix
  - Missing dependencies
  - Type errors

Please review and fix manually, then run `/bugfix.verify` again.
```

**Coverage Regression:**
```
⚠️  Code coverage decreased

Before: [old]%
After: [new]%

Consider adding more tests to cover:
  - [uncovered area 1]
  - [uncovered area 2]
```

---

## Examples

**Example 1: All Checks Pass**
```
User: /bugfix.verify

Agent: Running comprehensive verification...

✅ All verification checks passed!

Test Results:
  ✅ All tests pass (28/28)
  ✅ Code quality review: No critical issues
  ✅ Linters pass
  ✅ Build successful

Coverage: 94.2%

Next step: Run `/bugfix.submit`
```

**Example 2: Minor Issues Found**
```
User: /bugfix.verify

Agent: Running comprehensive verification...

⚠️  Verification completed with warnings:

Test Results:
  ✅ All tests pass (15/15)

Code Quality:
  ⚠️  1 warning: Consider extracting validation logic into separate function

Linters:
  ⚠️  3 formatting issues - Fixed automatically ✅

Build:
  ✅ Build successful

Overall: Ready to proceed with minor improvements noted.

Next step: Run `/bugfix.submit` (warnings are non-blocking)
```

**Example 3: Critical Issues**
```
User: /bugfix.verify

Agent: Running comprehensive verification...

❌ Verification failed:

Test Results:
  ❌ 2 tests failing (13/15)
    - test_edge_case_null_input: AssertionError
    - test_concurrent_access: RaceConditionError

These tests need to pass before proceeding.

Options:
  1. Run `/bugfix.fix` to attempt fixes
  2. Review failures manually
```
