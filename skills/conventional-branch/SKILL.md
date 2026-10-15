---
name: conventional-branch
description: Create or validate Git branch names using a local Conventional Branch variant with purpose prefixes only, including perf/ and its performance/ alias. Use when creating, naming, or checking a branch; never identify the AI tool in the branch prefix.
license: CC BY 4.0
metadata:
  version: "1.1.0-local.1"
  compatibility: "Requires git. External validators need configuration for this local variant."
  spec: https://conventionalbranch.org
  source: https://github.com/conventional-branch/conventional-branch
---

# Conventional Branch

Create Git branches using a purpose-only local adaptation of [Conventional Branch 1.1.0](https://conventionalbranch.org), with perf/performance support and no AI source prefixes.

## Branch Name Format

```
<type>/<description>
```

| Component | Format | Required | Example |
|-----------|--------|----------|---------|
| `type` | Defined prefix (see table below) | Yes | `feature`, `fix` |
| `description` | kebab-case, lowercase | Yes | `add-login-page` |

### Branch Types

Prefer full names over aliases, except performance work: prefer `perf/`; accept `performance/` as its alias. This is a local variant, not the unmodified upstream specification.

**Purpose prefixes** — describe the intent of the work:

| Type | Alias | Purpose |
|------|-------|---------|
| `feature/` | `feat/` | New features or enhancements |
| `bugfix/` | `fix/` | Bug fixes |
| `hotfix/` | — | Urgent production fixes |
| `release/` | — | Release preparation (dots allowed in version: `release/v1.2.0`) |
| `chore/` | — | Non-code tasks (deps, docs, config) |
| `perf/` | `performance/` | Performance improvements and optimization |

Choose prefixes by purpose, regardless of who or what creates the branch. Reject AI/tool source prefixes, including `ai/`, `copilot/`, `cursor/`, `claude/`, `codex/`, and nested forms such as `codex/perf/optimize-query`. Do not rename existing branches unless requested.

### Trunk Branches

`main`, `master`, and `develop` are trunk branches — they do not use a prefix. Never create new branches with the same names as trunk branches; branch off them instead.

## Naming Rules (Strict)

Validate against these local rules. External validators need configuration to match this variant:

- **Lowercase only** — no uppercase letters anywhere (`a-z` only)
- **Alphanumerics, hyphens, and dots** — `a-z`, `0-9`, `-`, `.`
- **Dots allowed only** in `release/` version descriptions (e.g., `release/v1.2.0`)
- **No underscores, spaces, or special characters**
- **No consecutive hyphens** (`--`), **dots** (`..`), or **hyphen-dot adjacency** (`-.` or `.-`)
- **No leading or trailing hyphens or dots** in the description

### Formal Grammar (ABNF)

```abnf
branch-name     = trunk-branch / prefixed-branch
trunk-branch    = "main" / "master" / "develop"
prefixed-branch = type "/" description / "release/" release-description
type            = "feature" / "feat" / "bugfix" / "fix"
                / "hotfix" / "chore" / "perf" / "performance"
description     = desc-segment *("-" desc-segment)
desc-segment    = 1*(ALPHA / DIGIT)
release-description = release-segment *("-" release-segment)
release-segment = desc-segment *("." desc-segment)
ALPHA           = %x61-7A
DIGIT           = %x30-39
```

### Valid Examples

```
main
master
develop
feature/add-login-page
feat/add-login-page
bugfix/fix-header-bug
fix/header-bug
hotfix/security-patch
release/v1.2.0
chore/update-dependencies
feature/issue-123-new-login
perf/optimize-query
performance/reduce-startup-time
```

### Invalid Examples (Do Not Produce These)

| Branch | Problem |
|--------|---------|
| `Feature/Add-Login` | Uppercase letters |
| `feature/new--login` | Consecutive hyphens |
| `feature/-new-login` | Leading hyphen |
| `feature/new-login-` | Trailing hyphen |
| `release/v1.-2.0` | Hyphen adjacent to dot |
| `fix/header bug` | Space |
| `fix/header_bug` | Underscore |
| `unknown/some-task` | Unknown prefix type |
| `codex/optimize-query` | AI source prefix |
| `ai/perf/optimize-query` | AI source prefix and extra slash |

## Description Guidelines

- Use **kebab-case** with 2-5 words
- Be descriptive but concise (~50 chars total)
- Good: `add-oauth-login`, `fix-header-overflow`, `update-ci-config`
- Bad: `fix-bug`, `new-feature`, `john-working-on-stuff`

## Workflow

Follow these steps when creating a new branch.

### 1. Gather Information

Ask the user for (if not already clear):

- **Branch type** — default to `feature` when uncertain
- **Brief description** — what the branch is for

Do not ask for a ticket number. If the user mentions one, include it in the description (e.g., `feature/issue-123-add-oauth`).

### 2. Validate the Name

Before creating, check the assembled name against the **Naming Rules** above. If any rule fails, fix it silently:

- Lowercase everything
- Replace underscores and spaces with hyphens
- Collapse consecutive hyphens
- Strip leading/trailing hyphens

### 3. Detect the Base Branch

Different repos use different trunk branches. Detect which one this repo uses:

```bash
# Prefer the remote's default branch
git symbolic-ref --short refs/remotes/origin/HEAD 2>/dev/null | sed 's|^origin/||'
```

If that returns nothing, check which trunk branch exists locally (priority order: `develop`, `main`, `master`):

```bash
for b in develop main master; do
  git show-ref --verify --quiet "refs/heads/$b" && echo "$b" && break
done
```

### 4. Create and Checkout

```bash
git checkout <base>
git pull origin <base>
git checkout -b <type>/<description>
```

### 5. Confirm

Tell the user:
- The branch name that was created
- That they are now on the new branch
- Remind them: `git push -u origin <branch-name>` when ready

## Validation with commit-check

For enforcement, commit-check is an option, but do not assume its defaults implement this variant. Configure and test accepted prefixes to include perf and performance and exclude AI source prefixes. This skill does not change hooks, CI, or hosting rules. The examples below do not prove local-policy enforcement:

```bash
# Check the current branch name
commit-check --branch
```

As a pre-commit hook, so a wrong name is caught at the first commit:

```yaml
repos:
  - repo: https://github.com/commit-check/commit-check
    rev: v2.18.0
    hooks:
      - id: check-branch
```

In GitHub Actions, [commit-check-action](https://github.com/commit-check/commit-check-action) runs the same check on every pull request (`branch: true` is its default). Copy-pasteable configuration for every platform, including a `commit-check.toml` that narrows the types to exactly the specification's, is at https://conventionalbranch.org/enforce/.

## Relationship with Conventional Commits

Conventional Branch is inspired by [Conventional Commits](https://www.conventionalcommits.org) and complements it naturally:

| Conventional Branch | Typical Conventional Commit |
|---------------------|----------------------------|
| `feature/add-login` | `feat: add login page` |
| `bugfix/fix-header` | `fix: header overflow on mobile` |
| `chore/update-deps` | `chore: bump lodash to 5.0` |
| `release/v1.2.0` | `chore: release v1.2.0` |

Align the branch type with commit types where possible (e.g., `feature/*` branches with `feat:` commits). See `conventional-commit` skill for commit message conventions.

## Attribution and Local Changes

Adapted from the Conventional Branch Contributors' skill at https://github.com/conventional-branch/conventional-branch/blob/main/skills/conventional-branch/SKILL.md under CC BY 4.0: https://creativecommons.org/licenses/by/4.0/. Conventional Branch credits Conventional Commits (https://www.conventionalcommits.org), by Conventional Changelog, under the same license.

Local changes: removed AI source prefix support, added perf/performance, aligned grammar with release-only dots, and clarified validator configuration. This is not an upstream specification change or endorsement.
