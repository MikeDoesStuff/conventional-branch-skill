# Install the customized Conventional Branch skill

## Requirements

- Install the customized skill from `MikeDoesStuff/conventional-branch-skill` on GitHub.
- Install in user-global scope for all supported agents, using symlinks to shared canonical content rather than independent copies.
- Keep purpose prefixes: feature/feat, bugfix/fix, hotfix, release, chore.
- Add preferred `perf/` and accepted alias `performance/`.
- Reject AI/tool source prefixes: ai, copilot, cursor, claude, codex, and nested forms.
- Preserve other naming rules and trunk handling. Do not rename existing branches or change unrelated skills.
- Use this standalone local skill; no repository fork or upstream checkout is needed.

## Work-PC procedure

1. Read local agent instructions. Inspect the installed skill, symlink/reparse-point target, installation scope, selected agents, and applicable skills lockfiles. The original command omitted `--global`; do not assume a global install. Resolve relative link targets against their parent directory.
2. Back up the installed skill and relevant metadata outside the replacement directory. Identify other consumers before editing a shared symlink target.
3. Verify read access to this public repository and review the remote skill before installing. Public installation should not require GitHub authentication. If corporate network policy blocks access, report the actual error rather than requesting or exposing credentials.
4. Check installed skills CLI help for current replacement/removal behavior. Back up and remove only conflicting conventional-branch installations from their discovered scopes. Project copies can shadow the requested global install: inspect the original project when known, but do not scan or alter unrelated projects. If the original project cannot be found, report that limitation. Never remove unrelated skills or use broad `--all` removal.

Install the specific skill globally for all agents supported by the installed CLI:

```powershell
npx skills add MikeDoesStuff/conventional-branch-skill --skill conventional-branch --global --agent '*' --yes
```

Do not use `--copy`. All agents means all CLI-supported agent targets, not only auto-detected installed applications. Some targets share a canonical directory; report these as shared-direct access rather than requiring redundant symlinks. Agent-specific directories should link to canonical content. On Windows, inspect LinkType/Target and resolve relative targets. If link creation fails or the installer falls back to copying, report the failure and missing prerequisite; do not silently accept copies or change Windows security settings.

5. Verify active content, resolved link paths, shared-direct access, and absence of competing upstream copies in inspected scopes. Confirm installation metadata records MikeDoesStuff/conventional-branch-skill, not conventional-branch/conventional-branch. Do not claim update safety without checking actual lockfile/source behavior. Reinstalling the upstream repository would restore upstream behavior.
6. Reload the affected agent if needed. Report paths, scope, agents, metadata changes, and checks. Do not commit, publish, push, or change branch enforcement unless requested.

Do not create or switch Git branches during installation or acceptance testing. Do not run bulk skill updates. Future updates should target this skill by name after verifying its recorded source still points to the customized repository.

## Acceptance checks

Evaluate names without creating or switching branches:

| Name | Expected |
|---|---|
| `feature/add-login-page`, `fix/header-overflow` | Accept |
| `perf/optimize-query` | Accept; preferred performance prefix |
| `performance/reduce-startup-time` | Accept as alias |
| `release/v1.2.0` | Accept |
| `main`, `master`, `develop` | Recognize existing trunks; do not create |
| `ai/optimize-query`, `copilot/add-login`, `cursor/fix-header` | Reject |
| `claude/security-patch`, `codex/optimize-query` | Reject |
| `codex/perf/optimize-query` | Reject |
| `perf/Optimize-Query`, `perf/optimize--query` | Reject |
| `perf/optimize.query`, `performance/-startup` | Reject |

Prompt check: request a name for optimizing a database query. Expect something like `perf/optimize-database-query`, never an AI source prefix. An explicitly requested `performance/` alias remains valid.

This changes agent guidance only. Hooks, CI, and hosting rules need separate configuration if they enforce another policy.

## Sources checked on 2026-10-15

- https://github.com/conventional-branch/conventional-branch/blob/main/skills/conventional-branch/SKILL.md
- https://github.com/vercel-labs/skills/blob/main/README.md
- https://github.com/conventional-branch/conventional-branch/blob/main/NOTICE
