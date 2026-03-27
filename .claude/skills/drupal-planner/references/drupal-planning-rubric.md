# Drupal Planning Quality Rubric

Quality checklist for evaluating drupal-planner output. Use this to verify plan completeness before handing off to implementation.

## Completeness Checks

### Entity Design (Required for any feature with entities)
- [ ] Every entity type has a one-sentence purpose
- [ ] Content vs config entity classification is justified
- [ ] Bundles are defined with rationale
- [ ] All fields specified: type, cardinality, required/optional, widget
- [ ] Entity relationship diagram present
- [ ] Relationships documented: direction, cardinality, reference type
- [ ] State ownership justified ("Entity X owns field Y because Z")

### Module Architecture (Required for any custom code)
- [ ] Contrib-first evaluation completed for every feature
- [ ] Each custom module has single, clear responsibility
- [ ] Plugin types identified (Block, Field, QueueWorker, etc.)
- [ ] Services defined with responsibilities and dependencies
- [ ] Hooks documented with when/why they run
- [ ] Module doesn't duplicate contrib functionality

### Configuration Schema (Required for any configurable feature)
- [ ] Every config item classified: simple config / config entity / State API
- [ ] Schema defined for each config item
- [ ] Config export strategy: config/install vs config/optional vs environment-specific
- [ ] Environment-sensitive values (API keys, URLs) excluded from export
- [ ] State API used for runtime data (not config)

### Permission & Access Model (Required for any user-facing feature)
- [ ] All permissions defined (view, create, edit, delete, admin)
- [ ] All roles mapped to permissions with rationale
- [ ] Workflow transitions mapped to roles (if applicable)
- [ ] Field-level access defined (if applicable)
- [ ] Edge cases addressed (multiple roles, permission revocation)

### Cache Strategy (Required for any rendered output)
- [ ] Every cacheable item identified
- [ ] Cache tags specified per item
- [ ] Cache contexts specified per item
- [ ] Max-age defined with rationale
- [ ] Invalidation triggers documented
- [ ] Dynamic Page Cache / BigPipe compatibility noted

### Migration & Update Path (Required for any data change)
- [ ] Source → target entity mapping complete
- [ ] Migrate API usage planned (source, process, destination plugins)
- [ ] Idempotency guaranteed (re-runnable without duplication)
- [ ] Rollback strategy documented
- [ ] hook_update_N sequence planned (if modifying existing)
- [ ] Drush commands for migration (if needed)

### Theme & Render (Required for any user-facing output)
- [ ] All rendered components identified with view modes
- [ ] Template files named
- [ ] Preprocess functions planned (display logic only)
- [ ] CSS/JS libraries defined
- [ ] Accessibility addressed (ARIA, semantic HTML, keyboard nav)

### Implementation Tasks (Always required)
- [ ] Tasks follow TDD rhythm (test first, implement, verify)
- [ ] Each task specifies exact files to create/modify
- [ ] Each task has test approach defined
- [ ] drupal-critic review checkpoints identified
- [ ] Tasks are sequenced correctly (dependencies respected)

## Calibration Guide

| Feature Complexity | Expected Plan Length | Required Sections |
|---|---|---|
| Simple (add field, minor config change) | 2-3 pages | Entity Design, Config Schema, Implementation Tasks |
| Medium (new content type with forms) | 5-8 pages | All except Migration (if not needed) |
| Complex (multi-entity with migration) | 10-15 pages | All sections, detailed |
| Critic fix (addressing REVISE findings) | 2-4 pages | Focused on specific architectural issues |

## Quality Signals

### Good Plan
- Every entity has purpose AND relationships
- Module decisions have documented evaluation
- Permission model has role→permission→rationale table
- Cache strategy has tags AND contexts AND invalidation
- Implementation tasks reference specific files
- Review checkpoints specify what drupal-critic should focus on

### Bad Plan
- Entity types listed without relationships or purpose
- "Add permissions during implementation" (deferred)
- "Cache this later" (deferred)
- Tasks are vague ("Create review system")
- No review checkpoints
- No migration or rollback strategy for data changes
