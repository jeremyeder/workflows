# /analyze - Root Cause Analysis

## Purpose
Perform systematic root cause analysis to identify the underlying issue causing the bug. This goes beyond symptoms to understand why the bug occurs and what needs to be fixed.

## Prerequisites
- Completed `/reproduce` command
- `artifacts/bugs/{bug-id}/reproduction.md` exists
- Reproduction test case available

## Arguments
None (uses most recent bug from workflow config)

## Process

1. **Review Reproduction Documentation**
   - Read `reproduction.md` thoroughly
   - Review reproduction test case
   - Understand the expected vs actual behavior
   - Identify affected code sections

2. **Code Investigation**
   - Examine the affected code sections in detail
   - Trace execution flow from entry point to error
   - Review recent changes (git history) that might have introduced the bug
   - Check for related code patterns that might have similar issues
   - Analyze data flow and state changes

3. **Hypothesis Formation**
   - Develop hypotheses about potential root causes
   - Consider multiple possibilities:
     - Logic errors
     - Race conditions or timing issues
     - Incorrect assumptions or edge cases
     - Missing validation or error handling
     - State management issues
     - Integration or dependency problems
     - Configuration errors
     - Resource constraints

4. **Testing Hypotheses**
   - For each hypothesis, trace the code path
   - Add debugging output or breakpoints (if applicable)
   - Analyze logs and error messages
   - Check boundary conditions and edge cases
   - Verify assumptions about data and state

5. **Root Cause Identification**
   - Determine the actual root cause
   - Distinguish between symptoms and underlying issues
   - Identify contributing factors
   - Assess impact and scope (how widespread is this issue?)

6. **Impact Analysis**
   - Identify all affected components or features
   - Determine potential side effects of fixing the bug
   - Check for similar patterns elsewhere in codebase
   - Assess risk of regression

7. **Document Analysis Findings**
   - Create `artifacts/bugs/{bug-id}/analysis.md` with:
     - Summary of findings
     - Root cause explanation
     - Technical details and code analysis
     - Contributing factors
     - Impact assessment
     - Recommended fix approach
     - Potential risks and considerations

## Output

### Files Created
- `artifacts/bugs/{bug-id}/analysis.md` - Comprehensive root cause analysis
- `artifacts/bugs/{bug-id}/logs/analysis-traces.log` - Debug traces (if applicable)

### Files Updated
- `.workflow-config.json` - Bug status updated to `ANALYZED`

## Usage Examples

```
/analyze
```

## Output Format

The `analysis.md` file will follow this structure:

```markdown
# Root Cause Analysis: {Bug Title}

**Bug ID:** {bug-id}
**Status:** ANALYZED
**Analyzed By:** {analyst}
**Date:** {date}

## Executive Summary
Brief overview of the root cause and recommended approach.

## Bug Overview
Quick recap of the bug from reproduction documentation.

## Investigation Process

### Code Examination
- **Affected Files:**
  - `path/to/file.js:123-145` - Description of issue
  - `path/to/another.py:67-89` - Related code

### Execution Flow Analysis
1. User triggers action at `controller.js:45`
2. Flow continues to `service.js:123`
3. Bug occurs at `helper.js:234` due to...

### Hypotheses Tested
1. **Hypothesis 1:** Null pointer exception
   - **Finding:** Ruled out - null checks are present
2. **Hypothesis 2:** Race condition in async handler
   - **Finding:** CONFIRMED - This is the root cause

## Root Cause

### Primary Cause
Detailed explanation of the root cause with code examples.

```javascript
// Problematic code
async function processRequest(data) {
  // Race condition: callback fires before validation completes
  validateData(data); // Returns promise but not awaited
  return processData(data); // Processes potentially invalid data
}
```

### Why This Causes the Bug
Explanation of how the root cause leads to the observed behavior.

### Contributing Factors
- Factor 1: Missing await keyword in async function
- Factor 2: Lack of validation error handling
- Factor 3: Assumption that validation is synchronous

## Impact Assessment

### Scope
- **Affected Components:** List of components/modules affected
- **User Impact:** How many users/features are impacted
- **Data Integrity:** Whether data could be corrupted
- **Security:** Any security implications

### Similar Issues
- `other-file.js:123` - Similar pattern that may need fixing
- `another-module.py:456` - Related code to review

## Recommended Fix Approach

### Solution Strategy
High-level description of how to fix the root cause.

### Implementation Steps
1. Add await to async validation call
2. Implement proper error handling
3. Add validation state check before processing
4. Add integration test for async validation flow

### Alternative Approaches Considered
1. **Approach A:** Make validation synchronous
   - **Pros:** Simpler logic
   - **Cons:** Performance impact
2. **Approach B:** Use callback pattern (REJECTED)
   - **Cons:** Callback hell, harder to maintain

### Risks and Considerations
- Risk 1: Changing async flow might affect other callers
- Risk 2: Error handling changes could alter user experience
- Mitigation: Add comprehensive tests, review all usage

## Code Sections Requiring Changes

### Primary Changes
- `service.js:123-145` - Add await and error handling
- `helper.js:234` - Update validation return handling

### Secondary Changes
- `tests/service.test.js` - Add async validation tests
- `similar-service.js:89` - Apply same fix to similar pattern

## Testing Strategy
- Unit tests for validation flow
- Integration tests for async scenarios
- Edge case tests for race conditions
- Performance tests to ensure no regression

## Additional Context
- Related documentation or architecture decisions
- Technical debt considerations
- Performance implications

## Next Steps
1. Proceed with `/fix` command to implement solution
2. Consider fixing similar patterns identified
3. Add monitoring/logging to prevent future occurrences
```

## Notes

- **Be thorough**: Don't settle for surface-level explanations
- **Question assumptions**: Verify what seems obvious
- **Consider context**: Understand the larger system architecture
- **Document reasoning**: Explain why you ruled out other causes
- **Think holistically**: Look for similar issues elsewhere
- **Be objective**: Focus on facts, not blame
- **Consider maintainability**: Think about long-term implications
