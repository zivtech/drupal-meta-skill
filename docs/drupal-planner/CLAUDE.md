# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A standalone **prompt-only** Claude Code skill package for planning Drupal implementations before coding. No build system, no runtime code, no dependencies — all deliverables are markdown files that install into `~/.claude/`.

This is the companion planner to [drupal-critic](https://github.com/zivtech/drupal-meta-skills). The planner designs architectures before code; the critic reviews implementations after code.

## Repository Structure

```
.claude/
  agents/
    drupal-planner.md                      # Main Drupal implementation planner (10-phase protocol)
    drupal-content-model-planner.md        # Drupal content model architecture planner
    drupal-taxonomy-planner.md             # Drupal taxonomy architecture planner
    drupal-theme-planner.md                # Drupal theme architecture planner
    drupal-search-planner.md               # Drupal search architecture planner
  skills/
    drupal-planner/
      SKILL.md                             # Main skill definition + sub-planner router
      references/
        contrib-evaluation-rubric.md       # Contrib vs custom decision framework
        drupal-planning-rubric.md          # Planning output quality checklist
        sub-planner-routing-map.md         # Sub-planner selection logic
    drupal-planner.content-model/
      SKILL.md                             # Content model sub-planner skill
    drupal-planner.taxonomy/
      SKILL.md                             # Taxonomy sub-planner skill
    drupal-planner.theme/
      SKILL.md                             # Theme sub-planner skill
    drupal-planner.search/
      SKILL.md                             # Search sub-planner skill
CLAUDE.md                                  # This file
AGENTS.md                                  # Agent registry
```

## Skill Architecture

### Main Planner: `/drupal-planner`

The main planner covers the full 10-phase Drupal implementation protocol:
1. Scope & Context
2. Existing Architecture Analysis
3. Data Model Design
4. Module Architecture
5. Configuration Schema
6. Permission & Access Model
7. Cache Strategy
8. Migration & Update Path
9. Theme & Render Design
10. Implementation Tasks & Review Checkpoints

It also acts as a **router**: when the request is clearly focused on a specific subsystem, it suggests the appropriate sub-planner.

### Sub-Planners: `/drupal-planner.<subsystem>`

Focused planners for specific Drupal subsystems, invoked via dot-notation:

| Command | Agent | Focus |
|---------|-------|-------|
| `/drupal-planner` | drupal-planner | Full implementation (all 10 phases) |
| `/drupal-planner.content-model` | drupal-content-model-planner | Entity types, bundles, paragraphs, fields, composition patterns |
| `/drupal-planner.taxonomy` | drupal-taxonomy-planner | Vocabularies, term hierarchies, facets, Views integration |
| `/drupal-planner.theme` | drupal-theme-planner | Base theme, components (SDC), CSS, preprocess, assets |
| `/drupal-planner.search` | drupal-search-planner | Search API, Solr/Elasticsearch, facets, discovery |

### Companion Critics

Each sub-planner has a companion critic for review after implementation:

| Planner | Companion Critic |
|---------|-----------------|
| drupal-planner | drupal-critic (this repo) |
| drupal-planner.content-model | content-model-critic (zivtech-meta-skills) |
| drupal-planner.taxonomy | taxonomy-critic (zivtech-meta-skills) |
| drupal-planner.theme | drupal-theme-critic (zivtech-meta-skills) |
| drupal-planner.search | search-discovery-critic (zivtech-meta-skills) |

## Key Design Decisions

- **Planning-only**: Planners produce architecture specifications, not implementation code.
- **Drupal-specific**: All sub-planners express domain knowledge in Drupal terms (entity API, Taxonomy module, Search API, theme layer), not CMS-agnostic terms.
- **Dot-notation sub-skills**: `/drupal-planner.content-model` etc. — one install, five commands.
- **Router pattern**: The main SKILL.md detects request focus and suggests sub-planners.
- **Evidence-backed**: Every architectural decision must include rationale. No "Drupal best practice" without specifics.
- **Hard gates**: Non-negotiable requirements that prevent vague designs (entity purpose, permission mapping, cache tags, migration idempotency).
- **Calibrated output**: Simple features get 2-3 pages; complex systems get 10-15 pages.

## Agent Prompt Format

All agent files use YAML frontmatter + XML body:

```yaml
---
name: agent-name
description: "..."
model: claude-opus-4-6
---

<Agent_Prompt>
  <Role> ... </Role>
  <Why_This_Matters> ... </Why_This_Matters>
  <Planning_Protocol> ... </Planning_Protocol>
  <Companion_Skills> ... </Companion_Skills>
  <Failure_Modes_To_Avoid> ... </Failure_Modes_To_Avoid>
  <Final_Checklist> ... </Final_Checklist>
</Agent_Prompt>
```

## When Editing Prompts

- Keep protocol phase order intact (phases build on each other)
- Preserve exact section headings in output format contracts — downstream tooling depends on them
- Maintain hard gates — removing them allows vague designs
- Preserve Realist Check and calibration guidance
- Sub-planners must stay Drupal-specific (not CMS-agnostic)

## Installation

```bash
# Install all skills and agents
cp -r .claude/skills/* ~/.claude/skills/
cp .claude/agents/*.md ~/.claude/agents/

# Or install via claude-skills
npx claude-skills add https://github.com/zivtech/drupal-meta-skills
```

## Drupal Architecture Principles Encoded

1. **Entity type design is high-consequence** — changing entity structure after content exists requires migrations
2. **Config vs state confusion breaks deployments** — every config item must be classified
3. **Contrib-first evaluation prevents maintenance burden** — use the contrib evaluation rubric
4. **Cache invalidation bugs are invisible until production load** — plan tags/contexts upfront
5. **Permission models need edge case handling** — plan for role combinations and permission revocation
6. **Theme layer should be thin** — business logic in preprocess is a maintenance nightmare
7. **hook_update_N ordering matters** — plan migrations as a state machine
8. **Search architecture determines discoverability** — index design precedes ranking tuning
