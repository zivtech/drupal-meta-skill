# AGENTS.md — drupal-meta-skills

| Agent | Type | Command | Companion / Role |
|-------|------|---------|------------------|
| drupal-planner | planner | `/drupal-planner` | Main Drupal architecture planner |
| drupal-content-model-planner | planner | `/drupal-planner.content-model` | Content model deep dive |
| drupal-taxonomy-planner | planner | `/drupal-planner.taxonomy` | Taxonomy deep dive |
| drupal-theme-planner | planner | `/drupal-planner.theme` | Theme architecture deep dive |
| drupal-search-planner | planner | `/drupal-planner.search` | Search architecture deep dive |
| drupal-critic | critic | `/drupal-critic` | Read-only Drupal review surface |
| drupal-config-executor | executor | `/drupal-config-executor` | Generates config YAML from planner specs |

## Notes

- All planner, critic, and executor surfaces are shipped from this repository.
- The config executor is the concrete generation step between planning and review.
