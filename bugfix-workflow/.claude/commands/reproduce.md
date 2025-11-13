# /reproduce - Reproduce and Document Bug

## Purpose
Set up the reproduction environment and systematically document the steps needed to reproduce the bug. This establishes a clear baseline for analysis and verification.

## Prerequisites
- Bug description or issue report
- Access to the affected codebase
- Understanding of the expected vs actual behavior

## Arguments
- **bug_description** (required): Brief description of the bug including context

## Process

1. **Create Bug Directory Structure**
   - Generate a unique bug ID based on description (e.g., `auth-login-failure`, `api-timeout-500`)
   - Create directory: `artifacts/bugs/{bug-id}/`
   - Create subdirectories: `logs/`, `screenshots/`, `tests/`

2. **Gather Bug Information**
   - Review the bug description provided
   - Search codebase for relevant code sections
   - Identify affected components, modules, or services
   - Collect error messages, stack traces, and logs
   - Document environment details (OS, browser, versions, dependencies)

3. **Reproduce the Bug**
   - Follow reported steps to reproduce
   - If steps are unclear, investigate and determine minimal reproduction steps
   - Document exact steps with input data and expected vs actual outcomes
   - Capture screenshots, logs, or error output
   - Note any conditions that affect reproducibility (timing, state, environment)

4. **Document Reproduction Details**
   - Create `artifacts/bugs/{bug-id}/reproduction.md` with:
     - Bug ID and title
     - Severity and priority assessment
     - Environment details
     - Minimal reproduction steps
     - Expected behavior
     - Actual behavior
     - Error messages and stack traces
     - Relevant code sections
     - Screenshots or logs (if applicable)
     - Reproduction rate (always/sometimes/rare)
     - Related issues or bugs

5. **Create Reproduction Test**
   - Write a failing test case that demonstrates the bug
   - Save to `artifacts/bugs/{bug-id}/tests/reproduction-test.{ext}`
   - Ensure the test reliably fails due to the bug

6. **Update Workflow Config**
   - Add bug entry to `.workflow-config.json`
   - Track bug status, created date, and assignee

## Output

### Files Created
- `artifacts/bugs/{bug-id}/reproduction.md` - Complete reproduction documentation
- `artifacts/bugs/{bug-id}/tests/reproduction-test.{ext}` - Failing test demonstrating the bug
- `artifacts/bugs/{bug-id}/logs/` - Directory for collected logs
- `.workflow-config.json` - Updated with bug tracking entry

### Status Update
- Bug status: `REPRODUCED`
- Ready for: `/analyze` command

## Usage Examples

```
/reproduce "Users unable to login with valid credentials on mobile devices"
```

```
/reproduce "API returns 500 error when pagination offset exceeds total records"
```

```
/reproduce "Memory leak in WebSocket connection handler after 1000+ messages"
```

## Output Format

The `reproduction.md` file will follow this structure:

```markdown
# Bug Report: {Bug Title}

**Bug ID:** {bug-id}
**Status:** REPRODUCED
**Severity:** [Critical|High|Medium|Low]
**Created:** {date}
**Reporter:** {name}

## Summary
Brief description of the bug and its impact.

## Environment
- **OS:**
- **Browser/Platform:**
- **Application Version:**
- **Dependencies:**
- **Configuration:**

## Steps to Reproduce
1. Step one with specific inputs
2. Step two
3. Step three
4. Observe the error

## Expected Behavior
What should happen when following the steps above.

## Actual Behavior
What actually happens (the bug).

## Error Messages
```
Paste error messages, stack traces, or logs here
```

## Affected Code Sections
- `path/to/file.js:123` - Description of relevance
- `path/to/another/file.py:456` - Description of relevance

## Reproduction Rate
- [ ] Always (100%)
- [ ] Frequently (>50%)
- [ ] Sometimes (10-50%)
- [ ] Rare (<10%)

## Screenshots/Logs
- Screenshot 1: `logs/screenshot-1.png`
- Log file: `logs/error.log`

## Related Issues
- Links to related bugs or issues
- Similar problems in the codebase

## Notes
Additional context, workarounds, or observations.
```

## Notes

- **Be thorough**: Accurate reproduction is critical for effective debugging
- **Minimal steps**: Find the shortest path to reproduce the bug
- **Isolate variables**: Identify which factors are necessary vs coincidental
- **Document everything**: Include details that seem obvious
- **Test reliability**: Ensure reproduction steps work consistently
- **Save artifacts**: Keep logs, screenshots, and error output for analysis
- **Safe to re-run**: Running this command again will update existing documentation
