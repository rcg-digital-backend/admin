# GitHub Copilot Instructions for Safe Settings Repository

## Repository Overview

This repository manages GitHub repository settings and policies using the Safe Settings framework. A GitHub App handles schema validation automatically - these instructions focus on YAML formatting and value validation for pull request reviews.

## Pull Request Review Responsibilities

When reviewing pull requests that modify YAML configuration files (`.github/suborgs/*.yml`, `.github/repos/*.yml`, `.github/settings.yml`):

### 1. YAML Formatting Checks

Verify proper YAML formatting:
- ✅ **2 space indentation** (not 4 spaces or tabs)
- ✅ **No tabs** (only spaces for indentation)
- ✅ **No trailing spaces** at end of lines

**Example - Correct Formatting:**
```yaml
repository:
  auto_init: true
  topics: [backend, services]
  private: true
```

**Example - Incorrect Formatting:**
```yaml
repository:
    auto_init: true    # ❌ 4 space indentation
	topics: [backend]  # ❌ Tab character
  private: true        # ❌ Trailing spaces
```

### 2. Value Validation Checks

#### Suborgrepos Conflict Detection

When a file is modified, check the `suborgrepos:` list doesn't clash with other settings files:

1. Read all `.github/suborgs/*.yml` and `.github/repos/*.yml` files
2. Compare repository patterns to detect overlaps
3. Patterns that could match the same repositories are conflicts

**Example Conflict:**
```yaml
# File: platform.yml
suborgrepos:
  - platform.*

# File: services.yml
suborgrepos:
  - platform.services.*  # ⚠️ Conflicts - both could match platform.services.api
```

#### Actor ID Validation

When `actor_id` appears in configurations (typically in `bypass_actors` or similar), ensure it's a **numeric integer**, not a string:

✅ **Correct:**
```yaml
bypass_actors:
  - actor_type: RepositoryRole
    actor_id: 5  # Numeric
  - actor_type: Team
    actor_id: 7737774  # Numeric
```

❌ **Incorrect:**
```yaml
bypass_actors:
  - actor_type: RepositoryRole
    actor_id: "admin"  # ❌ String instead of number
  - actor_type: Team
    actor_id: "my-team"  # ❌ String instead of team ID
```

**Common Actor IDs:**
- `RepositoryRole` admin: `5`
- `RepositoryRole` maintain: `4`
- `OrganizationAdmin`: `1`
- `Team`: Use numeric team ID (find with: `gh api /orgs/<org>/teams/<team-slug> --jq '.id'`)
- `Integration`: Use numeric GitHub App ID

### 3. PR Comment Format

#### If Issues Found

Leave a detailed comment with all issues:

```markdown
## ⚠️ Configuration Issues Found

### YAML Formatting Issues
- **Line 15**: Uses 4 space indentation instead of 2 spaces
- **Line 23**: Contains tab character instead of spaces
- **Line 42**: Has trailing whitespace

### Value Issues
- **Line 67**: `actor_id` must be numeric, not a string
  - Found: `actor_id: "admin"`
  - Expected: `actor_id: 5`

### Suborgrepos Conflicts
- Pattern `platform.*` conflicts with [.github/suborgs/platform.yml](../.github/suborgs/platform.yml)
  - Both files could match repositories like `platform.api`

**Please fix these issues before merging.**
```

#### If All Checks Pass

Leave a congratulatory comment:

```markdown
## ✅ Configuration Looks Great!

Nice work! All checks passed:
- ✅ YAML formatting is correct (2 space indentation, no tabs, no trailing spaces)
- ✅ All `actor_id` values are numeric
- ✅ No `suborgrepos` conflicts detected

Ready to merge! 🎉
```

## Helpful Commands

When suggesting fixes to users:

```bash
# Find team numeric IDs
gh api /orgs/rcg-digital-backend/teams/<team-slug> --jq '.id'

# Find user/app numeric IDs
gh api /users/<username> --jq '.id'

# Check YAML formatting with yamllint
yamllint .github/suborgs/*.yml
```

## Documentation References

- [Safe Settings Documentation](https://github.com/github/safe-settings)
- [GitHub REST API - Repository Rulesets](https://docs.github.com/rest/repos/rules)
- [GitHub REST API - Branch Protection](https://docs.github.com/rest/branches/branch-protection)
