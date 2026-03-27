# drupal-meta-skill

Drupal architecture planner-critic-executor ecosystem for Claude Code.

This consolidated repository packages:

- `drupal-planner`
- `drupal-planner.content-model`
- `drupal-planner.taxonomy`
- `drupal-planner.theme`
- `drupal-planner.search`
- `drupal-critic`
- `drupal-config-executor`

## Install

```bash
npx claude-skills add https://github.com/zivtech/drupal-meta-skill
```

## Commands

- `/drupal-planner`
- `/drupal-planner.content-model`
- `/drupal-planner.taxonomy`
- `/drupal-planner.theme`
- `/drupal-planner.search`
- `/drupal-critic`
- `/drupal-config-executor`

## Workflow

1. Use a planner command to design the Drupal architecture.
2. Use `drupal-config-executor` when you need concrete YAML generated from the planner spec.
3. Use `drupal-critic` to review the implementation or generated artifacts.

## Repository Layout

```text
.claude/
  agents/
  skills/
docs/
research/
scripts/
templates/
.github/workflows/
```

Skills and agents live at root `.claude/` for discovery. Supporting docs, research, and supply-chain tooling are bundled with the repo.

## License

GPL-3.0-or-later. See [LICENSE](LICENSE).
