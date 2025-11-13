# /test - Create Regression Tests and Validate Fix

## Purpose
Create comprehensive regression tests, execute all tests, and validate that the fix works correctly without introducing new issues.

## Prerequisites
- Completed `/fix` command
- `artifacts/bugs/{bug-id}/fix.md` exists
- Code changes have been implemented

## Arguments
None (uses most recent bug from workflow config)

## Process

1. **Review Fix Implementation**
   - Read `fix.md` to understand changes
   - Review modified code files
   - Understand the expected behavior after the fix

2. **Create Regression Tests**
   - Write test that reproduces the original bug scenario
   - Test should pass with the fix, would fail without it
   - Include edge cases identified during analysis
   - Test boundary conditions
   - Test error handling paths
   - Consider concurrency/timing issues if applicable

3. **Update Existing Tests**
   - Identify tests affected by the fix
   - Update test expectations if behavior changed
   - Ensure no tests were inadvertently broken
   - Update test data or mocks if needed

4. **Create Test Suite for Bug**
   - Organize tests in `artifacts/bugs/{bug-id}/tests/`
   - Include:
     - Unit tests for fixed functions
     - Integration tests for affected workflows
     - Edge case tests
     - Performance tests (if relevant)
     - Security tests (if relevant)

5. **Execute Test Suite**
   - Run all project tests (not just new ones)
   - Run linters and static analysis
   - Run type checkers (if applicable)
   - Execute integration tests
   - Run end-to-end tests (if applicable)

6. **Verify Test Coverage**
   - Check code coverage for modified files
   - Ensure fixed code paths have high coverage
   - Identify any untested code paths

7. **Manual Testing (if needed)**
   - Test the fix in development environment
   - Follow original reproduction steps
   - Test edge cases manually
   - Verify user experience is as expected
   - Test in different environments/browsers if relevant

8. **Performance Testing**
   - Compare performance before/after fix
   - Run benchmarks for affected code paths
   - Ensure no performance regression
   - Load test if applicable

9. **Document Test Results**
   - Create `artifacts/bugs/{bug-id}/tests/test-results.md`
   - Include:
     - Test execution summary
     - Coverage reports
     - Performance benchmarks
     - Manual testing results
     - Screenshots or evidence of fix working

10. **Create Test Report**
    - Summarize all testing activities
    - Document any issues found
    - Note any remaining concerns
    - Provide recommendations

## Output

### Files Created
- `artifacts/bugs/{bug-id}/tests/test-results.md` - Comprehensive test results
- `artifacts/bugs/{bug-id}/tests/regression-test.{ext}` - Regression test suite
- `artifacts/bugs/{bug-id}/tests/coverage/` - Coverage reports
- `artifacts/bugs/{bug-id}/tests/screenshots/` - Manual testing evidence

### Files Updated
- Existing test files with updates
- `.workflow-config.json` - Bug status updated to `TESTED`

## Usage Examples

```
/test
```

## Output Format

The `test-results.md` file will follow this structure:

```markdown
# Test Results: {Bug Title}

**Bug ID:** {bug-id}
**Status:** TESTED
**Tested By:** {tester}
**Date:** {date}
**Test Duration:** {duration}

## Executive Summary

**Overall Result:** ✅ PASS / ❌ FAIL
- Total Tests: 127
- Passed: 127
- Failed: 0
- Skipped: 0
- Coverage: 94.2%

The fix successfully resolves the bug and all tests pass. No regressions detected.

## Regression Tests

### Test Suite: Bug Reproduction
**Location:** `artifacts/bugs/{bug-id}/tests/regression-test.js`

| Test Case | Status | Duration | Notes |
|-----------|--------|----------|-------|
| Original bug scenario | ✅ PASS | 45ms | Previously failed, now passes |
| Edge case: empty data | ✅ PASS | 12ms | |
| Edge case: large dataset | ✅ PASS | 234ms | |
| Concurrent requests | ✅ PASS | 156ms | Race condition test |
| Invalid input handling | ✅ PASS | 23ms | |

### Test Code Examples

```javascript
describe('Bug #{bug-id}: Async validation race condition', () => {
  test('should properly await validation before processing', async () => {
    const validData = { userId: '123', action: 'update' };
    const result = await processRequest(validData);
    expect(result.status).toBe('success');
  });

  test('should handle validation errors correctly', async () => {
    const invalidData = { userId: null, action: 'update' };
    await expect(processRequest(invalidData))
      .rejects.toThrow(ValidationError);
  });

  test('should handle concurrent requests without race conditions', async () => {
    const requests = Array(10).fill(validData).map(processRequest);
    const results = await Promise.all(requests);
    expect(results).toHaveLength(10);
    expect(results.every(r => r.status === 'success')).toBe(true);
  });
});
```

## Unit Tests

### Modified Tests
- `tests/service.test.js` - 3 tests updated for new error handling
- `tests/helper.test.js` - 2 tests updated for async behavior

### New Tests
- `tests/service.test.js` - 5 new tests for async validation
- `tests/integration/validation.test.js` - 4 new integration tests

### Results
```
Service Tests
  ✓ processRequest with valid data (45ms)
  ✓ processRequest with invalid data throws ValidationError (23ms)
  ✓ processRequest awaits validation (12ms)
  ✓ processRequest handles validation timeout (1234ms)
  ✓ processRequest concurrent handling (156ms)

Helper Tests
  ✓ validateData returns promise (5ms)
  ✓ validateData rejects on invalid data (8ms)

Total: 7 passed, 0 failed
```

## Integration Tests

### Test Scenarios
1. **End-to-end API request flow**
   - Status: ✅ PASS
   - Validates entire request pipeline works correctly

2. **Multi-user concurrent scenario**
   - Status: ✅ PASS
   - Ensures no race conditions under load

3. **Error propagation through layers**
   - Status: ✅ PASS
   - Verifies errors are properly handled at all layers

### Results
```
Integration Tests
  ✓ API request with valid data returns 200 (234ms)
  ✓ API request with invalid data returns 400 (123ms)
  ✓ Concurrent API requests succeed (567ms)
  ✓ Validation errors return proper error messages (89ms)

Total: 4 passed, 0 failed
```

## Code Coverage

### Coverage Summary
| File | Statements | Branches | Functions | Lines |
|------|-----------|----------|-----------|-------|
| service.js | 98.2% | 95.0% | 100% | 98.0% |
| helper.js | 100% | 100% | 100% | 100% |
| **Overall** | **94.2%** | **91.5%** | **96.3%** | **93.8%** |

### Coverage Report
- All modified code paths are covered
- Edge cases have test coverage
- Error handling paths tested

### Uncovered Lines
- `service.js:234` - Deprecated code path (scheduled for removal)

## Performance Testing

### Benchmarks
| Scenario | Before Fix | After Fix | Delta | Status |
|----------|-----------|-----------|-------|--------|
| Single request | 45ms | 46ms | +1ms (+2%) | ✅ OK |
| 10 concurrent | 123ms | 125ms | +2ms (+1.6%) | ✅ OK |
| 100 concurrent | 1.2s | 1.23s | +30ms (+2.5%) | ✅ OK |
| Invalid data | 12ms | 13ms | +1ms (+8%) | ✅ OK |

**Conclusion:** Minimal performance impact, within acceptable range (<5%).

### Load Testing
- Sustained 1000 req/s for 5 minutes: ✅ PASS
- No memory leaks detected
- CPU usage unchanged

## Manual Testing

### Test Environment
- **OS:** macOS 13.4
- **Browser:** Chrome 114, Firefox 115, Safari 16
- **Node Version:** 18.16.0

### Test Scenarios

#### Scenario 1: Original Bug Reproduction Steps
1. Navigate to login page
2. Enter valid credentials
3. Click login button
4. **Expected:** Successful login
5. **Actual:** ✅ Successful login (bug fixed)

#### Scenario 2: Edge Cases
- Empty input fields: ✅ Shows validation error
- Special characters: ✅ Properly sanitized
- Network delay: ✅ Shows loading state

### Screenshots
- `screenshots/successful-login.png` - Fix working correctly
- `screenshots/validation-error.png` - Error handling working

## Cross-Browser Testing
| Browser | Version | Status | Notes |
|---------|---------|--------|-------|
| Chrome | 114 | ✅ PASS | Works perfectly |
| Firefox | 115 | ✅ PASS | Works perfectly |
| Safari | 16 | ✅ PASS | Works perfectly |
| Edge | 114 | ✅ PASS | Works perfectly |

## Static Analysis

### Linter
```
✓ No linting errors
✓ No warnings
```

### Type Checking (TypeScript/Flow)
```
✓ No type errors
✓ All types correct
```

### Security Scan
```
✓ No security vulnerabilities detected
✓ Input validation improved
```

## Regression Testing

### Existing Functionality
- All existing tests pass: ✅ (127/127)
- No unexpected failures
- No behavior changes in unrelated features

### Related Features
- User authentication: ✅ Working
- Session management: ✅ Working
- API endpoints: ✅ Working

## Issues Found During Testing

### Issue 1: [If any]
- **Description:**
- **Severity:**
- **Status:**
- **Resolution:**

*No issues found during testing.*

## Test Environment Details

### Dependencies
- Jest 29.5.0
- Testing Library 13.4.0
- Supertest 6.3.3

### Configuration
- Node.js 18.16.0
- npm 9.5.1
- Test timeout: 30s

## Recommendations

1. ✅ Fix is ready for deployment
2. ✅ All tests pass with good coverage
3. ✅ No performance regressions
4. ✅ Cross-browser compatibility verified
5. Consider adding monitoring for validation error rates

## Next Steps
1. Proceed with `/verify` for final verification and documentation
2. Create pull request with test results
3. Schedule deployment
```

## Notes

- **Test the fix thoroughly**: Don't skip edge cases
- **Regression test is critical**: Ensure the same bug can't happen again
- **Test the whole system**: Not just the fixed code
- **Document everything**: Future developers need to understand what was tested
- **Automate where possible**: Manual testing should supplement, not replace automated tests
- **Performance matters**: Ensure the fix doesn't slow things down
- **Think like a user**: Test real-world scenarios
- **Cross-browser/platform**: Test in relevant environments
