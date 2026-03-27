# AGENTS.md — Drupal Planner Agent Registry

## Overview

This repository contains 5 Drupal planning agents and 1 executor, organized as a main planner + 4 focused sub-planners + an executor that generates config YAML from planner specs. Planning agents are Opus tier and produce architecture specifications. The executor is Opus tier and produces Drupal configuration YAML.

## Agent Registry

### Planners

| Agent | Type | Model | Skill Command | Companion Critic |
|-------|------|-------|---------------|-----------------|
| drupal-planner | planner | opus | `/drupal-planner` | drupal-critic |
| drupal-content-model-planner | planner | opus | `/drupal-planner.content-model` | content-model-critic |
| drupal-taxonomy-planner | planner | opus | `/drupal-planner.taxonomy` | taxonomy-critic |
| drupal-theme-planner | planner | opus | `/drupal-planner.theme` | drupal-theme-critic |
| drupal-search-planner | planner | opus | `/drupal-planner.search` | search-discovery-critic |

### Executors

| Agent | Type | Model | Skill Command | Upstream Planner | Downstream Critic |
|-------|------|-------|---------------|-----------------|-------------------|
| drupal-config-executor | executor | opus | `/drupal-config-executor` | drupal-planner.content-model, .taxonomy, .search | content-model-critic, drupal-critic |

## Architecture

```
                        /drupal-planner (main)
                    Full 10-phase Drupal implementation
                    Routes to sub-planners when focused
                              │
         ┌────────────────────┼────────────────────┐
         │                    │                     │
         ▼                    ▼                     ▼
  .content-model         .taxonomy              .theme
  Entity types,        Vocabularies,         Base theme,
  bundles,             term hierarchies,     components,
  paragraphs,          facets,               CSS, preprocess,
  fields               Views integration     assets
         │                    │                     │
         │                    ▼                     │
         │               .search                    │
         │           Search API,                    │
         │           Solr/ES,                       │
         └──────────▶faceted search◀───────────────┘
                     discovery
                        │
                        ▼
              drupal-config-executor
              Generates config YAML from
              planner specs (config/install/)
                        │
                        ▼
              content-model-critic +
              drupal-critic (review)

Planner → Executor → Critic feedback loop:
  drupal-planner.* designs → drupal-config-executor generates → critics review
```

## Planner-Critic Companion Map

```
drupal-planner ─────────────────────────────▶ drupal-critic (this repo)
drupal-content-model-planner ───────────────▶ content-model-critic (meta-skills)
drupal-taxonomy-planner ────────────────────▶ taxonomy-critic (meta-skills)
drupal-theme-planner ───────────────────────▶ drupal-theme-critic (meta-skills)
drupal-search-planner ──────────────────────▶ search-discovery-critic (meta-skills)
```

## Planner-Executor-Critic Loop

```
drupal-planner.content-model ──▶ drupal-config-executor ──▶ content-model-critic
drupal-planner.taxonomy ───────▶ drupal-config-executor ──▶ drupal-critic
drupal-planner.search ─────────▶ drupal-config-executor ──▶ drupal-critic
```

## Sub-Planner Interaction Patterns

Sub-planners often work together for complex projects:

### New Site Build
1. `.content-model` → Design entity types and field architecture
2. `.taxonomy` → Design classification system
3. `.search` → Design search and discovery
4. `.theme` → Design theme architecture
5. Main `drupal-planner` → Tie together with permissions, cache, migrations

### Content Model Redesign
1. `.content-model` → Redesign entities and fields
2. `.taxonomy` → Update taxonomy to match new model
3. Main → Plan migration from old to new model

### Search Overhaul
1. `.search` → Design new search architecture
2. `.taxonomy` → Align taxonomies with facet requirements
3. Main → Plan implementation tasks and review checkpoints

## File Locations

```
.claude/agents/drupal-planner.md                                    # Main planner agent
.claude/agents/drupal-content-model-planner.md                      # Content model agent
.claude/agents/drupal-taxonomy-planner.md                           # Taxonomy agent
.claude/agents/drupal-theme-planner.md                              # Theme agent
.claude/agents/drupal-search-planner.md                             # Search agent
.claude/agents/drupal-config-executor.md                            # Config executor agent
.claude/skills/drupal-planner/SKILL.md                              # Main skill + router
.claude/skills/drupal-planner/references/contrib-evaluation-rubric.md
.claude/skills/drupal-planner/references/drupal-planning-rubric.md
.claude/skills/drupal-planner/references/sub-planner-routing-map.md
.claude/skills/drupal-planner.content-model/SKILL.md                # Content model sub-skill
.claude/skills/drupal-planner.taxonomy/SKILL.md                     # Taxonomy sub-skill
.claude/skills/drupal-planner.theme/SKILL.md                        # Theme sub-skill
.claude/skills/drupal-planner.search/SKILL.md                       # Search sub-skill
.claude/skills/drupal-config-executor/SKILL.md                      # Config executor skill
```

## External Dependencies

- [drupal-critic](https://github.com/zivtech/drupal-meta-skill) — Companion critic (this repo)
- [zivtech-meta-skills](https://github.com/zivtech/zivtech-meta-skills) — Source for companion critics (content-model-critic, taxonomy-critic, drupal-theme-critic, search-discovery-critic)
- [obra/superpowers](https://github.com/obra/superpowers) — brainstorming, plan writing, TDD, verification
- [flonat/claude-research](https://github.com/flonat/claude-research) — code archaeology
