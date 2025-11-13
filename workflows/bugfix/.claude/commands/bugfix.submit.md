---
description: Automatically commit, push, and create a pull request
displayName: Submit PR
icon: 🚀
---

## Submit Pull Request

Automatically commit changes, push to remote, and create a pull request.

---

## Execution Steps

### 1. Verify Prerequisites

Check that previous steps are complete:

```bash
# Find bug directory
BUG_DIR=$(ls -td artifacts/bugfixes/*/ | head -1)

# Check required files exist
if [ ! -f "$BUG_DIR/analysis.md" ]; then
  echo "Error: Missing analysis.md"
  exit 1
fi

if [ ! -f "$BUG_DIR/implementation.md" ]; then
  echo "Error: Missing implementation.md"
  exit 1
fi

if [ ! -f "$BUG_DIR/test-results.md" ]; then
  echo "Error: Missing test-results.md"
  exit 1
fi
```

If any file is missing:
```
Error: Incomplete workflow detected.

Missing steps:
  [List missing steps]

Please complete all steps before submitting:
  1. /bugfix.start - Analyze bug
  2. /bugfix.fix - Implement fix
  3. /bugfix.verify - Verify with tests
  4. /bugfix.submit - Submit PR (you are here)
```

### 2. Read Workflow Artifacts

Load all context from previous steps:

```bash
# Read all artifacts
BUG_ID=$(basename "$BUG_DIR")
ANALYSIS=$(cat "$BUG_DIR/analysis.md")
IMPLEMENTATION=$(cat "$BUG_DIR/implementation.md")
TEST_RESULTS=$(cat "$BUG_DIR/test-results.md")

# Extract key information
BUG_SUMMARY=$(grep "## Bug Description" -A 5 "$BUG_DIR/analysis.md" | tail -4)
SOURCE_REF=$(grep "^- Reference:" "$BUG_DIR/analysis.md" | cut -d' ' -f3-)
CHANGED_FILES=$(grep "^- \`" "$BUG_DIR/implementation.md" | sed 's/^- `\(.*\)`.*/\1/')
```

### 3. Stage Changes

Add all changes to git:

```bash
# Get list of modified files
git add -A

# Show what will be committed
git status --short
```

### 4. Create Commit

Generate a comprehensive commit message:

```markdown
fix: [Bug summary - one line]

[Detailed description of the bug and fix]

## Bug
[Bug description from analysis]

## Root Cause
[Root cause from analysis]

## Changes
[List of changes from implementation]

## Testing
[Test results summary]

## References
[GitHub issue, Jira ticket, or bug report reference]

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

Commit the changes:

```bash
# Create commit using heredoc for proper formatting
git commit -m "$(cat <<'EOF'
fix: [summary]

[full commit message as above]
EOF
)"
```

### 5. Push to Remote

Push the branch to the remote repository:

```bash
# Get current branch name
BRANCH=$(git branch --show-current)

# Push with upstream tracking
git push -u origin "$BRANCH"

# Capture push result
PUSH_STATUS=$?
```

If push fails:
```
Error: Failed to push to remote.

Common issues:
  - Not authenticated with GitHub: gh auth login
  - Remote branch exists: git pull --rebase origin [branch]
  - No remote configured: git remote add origin [url]

Please resolve and run `/bugfix.submit` again.
```

### 6. Create Pull Request

Use GitHub CLI to create PR:

```bash
# Extract PR title from commit message
PR_TITLE=$(git log -1 --pretty=format:%s)

# Create PR body
PR_BODY=$(cat <<'EOF'
## Summary

[Brief description of the bug fix]

## Bug Analysis

[Key points from analysis.md]

## Changes Made

[List of file changes from implementation.md]

## Testing

[Test results summary from test-results.md]

## References

[Links to GitHub issues, Jira tickets, etc.]

---

## Verification Checklist

- [✅] Tests pass
- [✅] Code quality review completed
- [✅] Linters pass
- [✅] Build successful
- [✅] No breaking changes

## Test Coverage

[Coverage metrics if available]

---

🤖 Generated with [Claude Code](https://claude.com/claude-code)
EOF
)

# Create the PR
gh pr create \
  --title "$PR_TITLE" \
  --body "$PR_BODY" \
  --base main

# Capture PR URL
PR_URL=$(gh pr view --json url -q .url)
```

### 7. Document PR Information

Create PR info document:

```markdown
# Pull Request: [BUG_ID]

## PR Details
- **Title**: [PR title]
- **URL**: [PR URL]
- **Branch**: [branch name]
- **Base**: main
- **Status**: Open

## Created
[Timestamp]

## Commits
[List of commits in the PR]

## Files Changed
[List of changed files]

## Reviewers
[Auto-assigned reviewers if any]

## Labels
[Auto-applied labels if any]

## CI/CD Status
[Link to CI/CD runs]

## Next Steps
- Wait for CI/CD checks to pass
- Request review from team members
- Address review feedback if needed
- Merge when approved
```

Save to: `artifacts/bugfixes/$BUG_ID/pr-info.md`

### 8. Complete Workflow

Update the todo list:

```
TodoWrite:
  [✅] Analyze bug
  [✅] Implement fix
  [✅] Verify with tests
  [✅] Create pull request
```

Present final summary:

```
✅ Pull request created successfully!

PR Details:
  Title: [PR title]
  URL: [PR URL]
  Branch: [branch name]

Changes:
  [Number] files modified
  [Number] tests added
  Coverage: [percentage]%

The PR has been submitted with:
  ✅ Automated tests
  ✅ Quality checks
  ✅ Comprehensive description

Next steps:
  1. CI/CD will run automatically
  2. Request review from your team
  3. Monitor PR: [PR URL]

Workflow complete! 🎉
```

---

## Error Handling

**No Changes to Commit:**
```
Error: No changes to commit.

Git status shows no modifications.

Possible reasons:
  - Changes were already committed
  - No files were actually modified
  - Changes were discarded

Please verify your changes and try again.
```

**Commit Fails:**
```
Error: Git commit failed.

[Git error message]

Common issues:
  - Pre-commit hooks failing
  - Commit message format invalid
  - Files locked by other process

Please resolve and run `/bugfix.submit` again.
```

**Push Fails (Authentication):**
```
Error: Push failed - authentication required.

Authenticate with GitHub:
  gh auth login

Then run `/bugfix.submit` again.
```

**Push Fails (Force Push Needed):**
```
Error: Push rejected - branch diverged.

This usually means the remote branch was modified.

Options:
  1. Pull and rebase: git pull --rebase origin [branch]
  2. Force push (dangerous): git push --force-with-lease

Recommended: Pull and rebase, then run `/bugfix.submit` again.
```

**PR Creation Fails:**
```
Error: Failed to create pull request.

[GitHub error message]

Common issues:
  - PR already exists for this branch
  - Missing permissions
  - Base branch doesn't exist

Check GitHub and resolve the issue.

The commit and push succeeded, so you can create the PR manually:
  Branch: [branch name]
  URL: https://github.com/[owner]/[repo]/pull/new/[branch]
```

**PR Already Exists:**
```
✅ Changes pushed successfully!

ℹ️  Note: A PR already exists for this branch.

Existing PR: [PR URL]

The new commits have been added to the existing PR.
No new PR was created.
```

---

## Examples

**Example 1: Successful PR Creation**
```
User: /bugfix.submit

Agent: Verifying prerequisites...
Agent: Staging changes...
Agent: Creating commit...
Agent: Pushing to remote...
Agent: Creating pull request...

✅ Pull request created successfully!

PR Details:
  Title: fix: Add input sanitization for special characters in usernames
  URL: https://github.com/myorg/myapp/pull/789
  Branch: bugfix/GH-456

Changes:
  2 files modified
  3 tests added
  Coverage: 94.2%

Next steps:
  1. CI/CD will run automatically
  2. Request review from your team
  3. Monitor PR: https://github.com/myorg/myapp/pull/789

Workflow complete! 🎉
```

**Example 2: With Pre-existing PR**
```
User: /bugfix.submit

Agent: Verifying prerequisites...
Agent: Staging changes...
Agent: Creating commit...
Agent: Pushing to remote...

✅ Changes pushed successfully!

ℹ️  Note: A PR already exists for this branch.

Existing PR: https://github.com/myorg/myapp/pull/788

The new commits have been added to the existing PR.
```

**Example 3: Authentication Required**
```
User: /bugfix.submit

Agent: Verifying prerequisites...
Agent: Staging changes...
Agent: Creating commit...
Agent: Pushing to remote...

❌ Error: Push failed - authentication required.

Please authenticate with GitHub:
  gh auth login

Then run `/bugfix.submit` again.
```

---

## Customization Options

You can customize the PR creation by setting environment variables:

```bash
# Set custom PR base branch
export BUGFIX_BASE_BRANCH="develop"

# Set custom PR labels
export BUGFIX_PR_LABELS="bug,automated-fix"

# Set custom reviewers
export BUGFIX_REVIEWERS="@team/reviewers,john.doe"

# Skip CI for draft PRs
export BUGFIX_DRAFT=true
```

These can be set in your shell profile or project configuration.
