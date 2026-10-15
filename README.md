# Conventional Branch - purpose-only variant

A standalone adaptation of the Conventional Branch agent skill. This repository contains only the customized skill and its installation documentation, not a fork of the upstream website or full repository.

## Local policy

- Keep `feature/` (`feat/`), `bugfix/` (`fix/`), `hotfix/`, `release/`, and `chore/`.
- Use `perf/` for performance improvements; accept `performance/` as an alias.
- Reject AI/tool source prefixes such as `ai/`, `copilot/`, `cursor/`, `claude/`, and `codex/`, including nested forms.
- Preserve ordinary naming rules and existing trunk handling.

The skill provides agent guidance. It does not change Git hooks, CI, or hosting rules.

## Install for all agents

```powershell
npx skills add MikeDoesStuff/conventional-branch-skill --skill conventional-branch --global --agent '*' --yes
```

This public standalone repository contains the adapted skill, not the full upstream project. Public read access does not require GitHub authentication.

The command selects only this skill for all agents supported by the installed skills CLI, in user-global scope. Do not pass `--copy`; verify symlinks after installation. Shared canonical skill directories may be used directly instead of through a separate link.

For an existing installation, give your local agent [LOCAL-AGENT-HANDOFF.md](LOCAL-AGENT-HANDOFF.md). It covers backup, old project/global copies, symlink verification, and source metadata.

## Install from a local checkout

```powershell
npx skills add ./skills --skill conventional-branch
```

For an existing installation, follow [LOCAL-AGENT-HANDOFF.md](LOCAL-AGENT-HANDOFF.md) to inspect its scope and symlinks, back it up, replace it, and verify installation metadata. Keep this checkout as your customized source instead of reinstalling the upstream skill.

The skill entrypoint is [skills/conventional-branch/SKILL.md](skills/conventional-branch/SKILL.md).

## License and attribution

This adaptation is distributed under the **Creative Commons Attribution 4.0 International license (CC BY 4.0)**. See [LICENSE](LICENSE) for the complete license text, [NOTICE](NOTICE) for attribution and modifications, and [UPSTREAM-NOTICE](UPSTREAM-NOTICE) for the upstream notice retained verbatim.

The original skill is by the Conventional Branch Contributors. Conventional Branch credits Conventional Commits by Conventional Changelog. This is an independent local adaptation, not an official upstream release or endorsement.

When sharing this adaptation, retain the attribution, license information, and indication that changes were made. No additional license restrictions are imposed by this repository.
