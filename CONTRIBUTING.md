# Contributing

## Scope

This is a prompt-only repository with supporting scripts, research assets, and GitHub workflows.

## Expectations

- Keep root `.claude/skills/` and `.claude/agents/` installable.
- Preserve the no-copy policy for third-party Drupal skills referenced by the critic manifest.
- Keep planner, executor, and critic contracts consistent with each other.
- Maintain the validation workflow and Python scripts when manifest structure changes.

## Verification

Before shipping changes:

1. Check required skill files still exist under root `.claude/skills/`.
2. Run `python scripts/verify_no_copied_skills.py`.
3. Run `python scripts/refresh_external_skills.py --check`.
4. Confirm docs and install instructions point to `https://github.com/zivtech/drupal-meta-skill`.
