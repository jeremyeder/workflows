# Bugfix Workflow

Automated bug-to-PR workflow that analyzes issues, implements fixes, runs tests, and creates pull requests with minimal human interaction.

## Purpose

This workflow automates the entire bug fixing process from analysis to PR creation, enabling engineers to:
- Quickly analyze and understand bugs
- Implement fixes using test-driven development
- Run comprehensive verification
- Create pull requests automatically

Perfect for routine bug fixes that don't require significant architectural changes.

---

## Quick Start

### 1. Start with a Bug

```bash
# From a GitHub issue
/bugfix.start https://github.com/owner/repo/issues/123
/bugfix.start #123

# From a Jira ticket
/bugfix.start PROJ-456

# From a description
/bugfix.start Login fails when username contains special characters
```

### 2. Implement the Fix

```bash
/bugfix.fix
```

This automatically:
- Creates failing tests that demonstrate the bug
- Implements the minimal fix to pass tests
- Refactors for maintainability

### 3. Verify the Fix

```bash
/bugfix.verify
```

This runs:
- Complete test suite
- Code quality review
- Linters and formatters
- Build verification

### 4. Submit a Pull Request

```bash
/bugfix.submit
```

This automatically:
- Commits changes with descriptive message
- Pushes to remote repository
- Creates pull request with full context
- Links to original issue/ticket

---

## Workflow Overview

```
┌─────────────────┐
│  /bugfix.start  │  Analyze bug (from GitHub, Jira, or text)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   /bugfix.fix   │  Implement fix using TDD
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ /bugfix.verify  │  Run tests and quality checks
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ /bugfix.submit  │  Create PR automatically
└─────────────────┘
```

---

## Commands

### /bugfix.start

**Purpose**: Analyze a bug and prepare for fixing

**Input Types**:
- GitHub issue URL: `https://github.com/owner/repo/issues/123`
- GitHub issue number: `#123` or `GH-123`
- Jira ticket: `PROJ-456` or full URL
- Plain text: Any bug description

**What It Does**:
1. Fetches issue/ticket details (if applicable)
2. Creates a dedicated bug workspace and Git branch
3. Uses code-explorer agent to find the bug location
4. Documents root cause and fix strategy
5. Creates analysis document

**Outputs**:
- `artifacts/bugfixes/{bug-id}/analysis.md`
- Git branch: `bugfix/{bug-id}`

---

### /bugfix.fix

**Purpose**: Implement the bug fix using test-driven development

**Prerequisites**: Must run `/bugfix.start` first

**What It Does**:
1. Reads the bug analysis
2. Uses tdd-developer agent to implement fix
3. Follows red-green-refactor cycle:
   - Write failing tests
   - Implement minimal fix
   - Refactor for quality
4. Runs quick sanity tests
5. Documents implementation

**Outputs**:
- Code changes (in working directory)
- Test files (in working directory)
- `artifacts/bugfixes/{bug-id}/implementation.md`

---

### /bugfix.verify

**Purpose**: Run comprehensive tests and quality checks

**Prerequisites**: Must run `/bugfix.fix` first

**What It Does**:
1. Runs complete test suite
2. Uses code-reviewer agent for quality review
3. Runs linters and formatters
4. Verifies build succeeds
5. Documents all results

**Quality Checks**:
- ✅ Tests pass
- ✅ Code quality (no critical issues)
- ✅ Security review
- ✅ Performance impact
- ✅ No breaking changes
- ✅ Linters pass
- ✅ Build succeeds

**Outputs**:
- `artifacts/bugfixes/{bug-id}/test-results.md`

---

### /bugfix.submit

**Purpose**: Automatically commit, push, and create a pull request

**Prerequisites**: Must run `/bugfix.verify` first with passing results

**What It Does**:
1. Stages all changes
2. Creates descriptive commit message
3. Commits changes
4. Pushes to remote repository
5. Creates pull request via GitHub CLI
6. Documents PR information

**PR Includes**:
- Clear title with "fix:" prefix
- Comprehensive description
- Bug analysis summary
- List of changes
- Test results
- References to original issue/ticket
- Verification checklist

**Outputs**:
- Git commit (on remote)
- GitHub pull request
- `artifacts/bugfixes/{bug-id}/pr-info.md`

---

## Agents Used

### code-explorer
- **When**: During `/bugfix.start`
- **Purpose**: Find bug location and understand context
- **Output**: List of key files to examine

### tdd-developer
- **When**: During `/bugfix.fix`
- **Purpose**: Implement fix using test-first approach
- **Output**: Tests and code changes

### code-reviewer
- **When**: During `/bugfix.verify`
- **Purpose**: Review fix quality and correctness
- **Output**: Quality assessment and issues found

---

## Input Support

### GitHub Issues

```bash
# Full URL
/bugfix.start https://github.com/myorg/myapp/issues/456

# Issue number (auto-detects from current repo)
/bugfix.start #456
/bugfix.start GH-456
```

Automatically fetches:
- Issue title
- Issue description
- Labels
- Comments (if relevant)

**Prerequisites**:
- `gh` CLI installed and authenticated
- Correct repository context

### Jira Tickets

```bash
# Ticket ID
/bugfix.start WEBAPP-789

# Full URL
/bugfix.start https://jira.company.com/browse/WEBAPP-789
```

**Note**: Jira integration is currently a placeholder. Full implementation requires:
- Jira API credentials
- Jira REST API client
- Custom configuration

### Plain Text

```bash
/bugfix.start User authentication fails when password contains Unicode characters
```

Best for:
- Internal bug reports
- Quick fixes
- Bugs without formal tickets

---

## Automation Level

This workflow is **fully automated**:

✅ **Auto-commit**: Changes committed automatically
✅ **Auto-push**: Branch pushed to remote automatically
✅ **Auto-PR**: Pull request created automatically

**Manual intervention only required for**:
- Reviewing the PR (after creation)
- Addressing PR feedback
- Merging the PR

**Optional manual intervention**:
- If verification fails, you can fix issues manually
- If you want to customize the PR before submission
- If you want to add additional changes

---

## Prerequisites

### Required Tools

1. **Git**: Version control
   ```bash
   git --version
   ```

2. **GitHub CLI**: For PR creation
   ```bash
   gh --version
   gh auth status
   ```

3. **Test Framework**: Project-specific
   - Node.js: `npm test` configured in package.json
   - Python: pytest, unittest, etc.
   - Go: `go test`
   - Others: Custom test command

### Authentication

Authenticate with GitHub:
```bash
gh auth login
```

Choose:
- GitHub.com
- HTTPS
- Authenticate via browser or token

---

## Customization

### Environment Variables

Customize behavior with environment variables:

```bash
# PR base branch (default: main)
export BUGFIX_BASE_BRANCH="develop"

# PR labels
export BUGFIX_PR_LABELS="bug,automated-fix,needs-review"

# PR reviewers
export BUGFIX_REVIEWERS="@team/backend,john.doe"

# Create as draft PR
export BUGFIX_DRAFT=true

# Skip CI runs
export BUGFIX_SKIP_CI=true
```

### Custom Test Commands

If your project uses a non-standard test command:

```bash
# Add to package.json
{
  "scripts": {
    "test": "your-test-command"
  }
}

# Or use Makefile
test:
	your-test-command
```

---

## Outputs

All artifacts are saved to `artifacts/bugfixes/{bug-id}/`:

```
artifacts/bugfixes/GH-456/
├── analysis.md         # Bug analysis and fix strategy
├── implementation.md   # Changes made and tests added
├── test-results.md     # Verification results
└── pr-info.md         # Pull request details
```

These artifacts are tracked in `ambient.json` results:
- **Bug Analysis**: Analysis documents
- **Fix Implementation**: Implementation summaries
- **Test Results**: Verification reports
- **Pull Request**: PR information

---

## Examples

### Example 1: GitHub Issue Fix

```bash
# Start
$ /bugfix.start #456
✅ Bug analysis complete!
Bug ID: GH-456
Root Cause: Null pointer in user validation

# Fix
$ /bugfix.fix
✅ Fix implemented!
Changed: src/auth/validator.js
Tests added: 3
All tests pass ✅

# Verify
$ /bugfix.verify
✅ All verification checks passed!
Coverage: 94.2%

# Submit
$ /bugfix.submit
✅ Pull request created!
URL: https://github.com/myorg/myapp/pull/789
```

### Example 2: Quick Text Fix

```bash
$ /bugfix.start API returns 500 when user_id is null
✅ Bug analysis complete!

$ /bugfix.fix
✅ Fix implemented!

$ /bugfix.verify
✅ Verification passed!

$ /bugfix.submit
✅ PR created!
```

### Example 3: Jira Ticket Fix

```bash
$ /bugfix.start WEBAPP-789
✅ Analyzing Jira ticket WEBAPP-789...
Note: Jira integration is placeholder - using ticket ID as reference

$ /bugfix.fix
✅ Fix implemented!

$ /bugfix.verify
✅ Verification passed!

$ /bugfix.submit
✅ PR created with reference to WEBAPP-789
```

---

## Troubleshooting

### Issue: "No bug analysis found"

**Cause**: Skipped `/bugfix.start`

**Solution**: Run `/bugfix.start {bug-info}` first

---

### Issue: "Tests failed"

**Cause**: Implementation didn't fix the bug or introduced regression

**Solution**:
1. Review test failures in `artifacts/bugfixes/{id}/test-results.md`
2. Run `/bugfix.fix` again (it will use failure context)
3. Or fix manually and run `/bugfix.verify` again

---

### Issue: "Push failed - authentication"

**Cause**: Not authenticated with GitHub

**Solution**:
```bash
gh auth login
```

---

### Issue: "PR creation failed"

**Cause**: Various (permissions, existing PR, etc.)

**Solution**:
- Check error message for specifics
- Verify `gh` CLI works: `gh pr list`
- Create PR manually if needed (commit/push already succeeded)

---

### Issue: "Linting errors"

**Cause**: Code doesn't meet project standards

**Solution**:
- Workflow attempts auto-fix first
- If auto-fix fails, review errors and fix manually
- Run `/bugfix.verify` again

---

## Best Practices

1. **Start with Good Input**: Provide clear bug descriptions or valid issue references
2. **Review Analysis**: Check the analysis document before proceeding to fix
3. **Trust but Verify**: Review the implementation even though it's automated
4. **Monitor PR**: Watch for CI/CD failures after PR creation
5. **Respond to Reviews**: Address feedback from human reviewers promptly

---

## Limitations

**Not Suitable For**:
- Large architectural changes (use feature workflow instead)
- Bugs requiring significant refactoring
- Bugs affecting multiple systems
- Bugs needing extensive research

**Best For**:
- Logic errors in existing code
- Input validation issues
- Edge case handling
- Small behavior corrections
- Test gap fixes

---

## Future Enhancements

Planned improvements:
- [ ] Full Jira API integration
- [ ] Automatic issue/ticket status updates
- [ ] Slack/email notifications on PR creation
- [ ] Integration with CI/CD for auto-merge on success
- [ ] Support for GitLab and other platforms
- [ ] Custom PR templates
- [ ] Automatic reviewer assignment based on CODEOWNERS

---

## Contributing

To enhance this workflow:
1. Test changes thoroughly
2. Update this README
3. Add examples
4. Submit PR with improvements
