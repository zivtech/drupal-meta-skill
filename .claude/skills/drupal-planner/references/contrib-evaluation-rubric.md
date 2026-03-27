# Contrib vs Custom Module Evaluation Rubric

Decision framework for evaluating whether to use a contrib module or build custom code. Apply this rubric for every module decision in a Drupal plan.

## Evaluation Criteria

| Criterion | Weight | Contrib Favorable | Custom Favorable |
|-----------|--------|-------------------|------------------|
| **Functionality match** | High | Module solves 80%+ of the need | Module solves <50% or requires heavy patching |
| **Security coverage** | High | SA-covered (security advisory process) | Not SA-covered, abandoned, or no security policy |
| **Maintenance activity** | High | Active maintainer, regular releases, responsive issue queue | No commits in 12+ months, large issue backlog |
| **Download count** | Medium | >5,000 installs (proven adoption) | <500 installs (unproven, risk of abandonment) |
| **Drupal version support** | High | Supports current Drupal version (D10/D11) | Only supports older versions, no upgrade path |
| **Issue queue health** | Medium | Low bug count, responsive maintainer | Many open critical/major bugs, unresponsive |
| **Code quality** | Medium | Follows Drupal coding standards, automated tests | Poor code quality, no tests, procedural spaghetti |
| **Performance impact** | Medium | Lightweight, no unnecessary overhead | Heavy, loads assets globally, many database queries |
| **Configuration flexibility** | Low | Configurable enough for the use case | Too rigid or too complex for the need |
| **Upstream contribution** | Low | Can contribute patches upstream | Fork would be hard to maintain |

## Decision Matrix

### Use Contrib When:
- Module is SA-covered with active maintenance
- Functionality match is 70%+ with reasonable configuration
- Module has >2,000 installs and supports current Drupal version
- Custom code would duplicate what contrib already does well
- The alternative is owning security updates for complex functionality (auth, encryption, payment)

### Build Custom When:
- No contrib module solves the core need (not just missing features)
- Available modules require >30% patching/overriding to work
- The functionality is simple enough that a custom module is less code than contrib + patches
- Performance requirements exceed what contrib modules can deliver
- The feature is truly unique to this project with no reuse potential

### Hybrid Approach (Contrib + Custom):
- Use contrib for the heavy lifting (entity management, workflows)
- Build a thin custom module for project-specific integration
- Document which contrib features are used and which are custom additions
- Plan for contrib updates: will custom code survive a contrib version bump?

## Documentation Template

For each module decision in a plan, document:

```
**Module decision: [feature]**
- Evaluated: [list of contrib modules considered]
- Selected: [contrib module name] / Custom module
- Rationale: [why this choice, using rubric criteria]
- If contrib: Version, SA-covered? Downloads? Last release?
- If custom: Why contrib doesn't solve it? Estimated maintenance burden?
- Risk: [what could go wrong with this choice]
```

## Red Flags (Automatic Custom Bias)

These patterns should make you strongly consider custom code:
- Contrib module requires forking to work
- Contrib module has known security issues without patches
- Contrib module pulls in 10+ dependencies for one feature
- Contrib module's architecture conflicts with the project's patterns
- Contrib module hasn't been updated for the current Drupal major version

## Red Flags (Automatic Contrib Bias)

These patterns should make you strongly consider contrib:
- Custom code would implement authentication, encryption, or payment processing
- Custom code would duplicate a well-maintained, SA-covered module
- Custom code would require ongoing security maintenance for complex functionality
- The feature is a standard CMS pattern (media, workflows, revisions, SEO)
