# Convention Over Configuration

Predetermined conventions replace explicit configuration. Strong defaults make common cases trivial while allowing customization when needed.

## Core Principle

When 90% of use cases share the same configuration, make that configuration the default. Users start working immediately without setup decisions. The system embeds expert knowledge into its structure, so users inherit good practices automatically.

## Benefits

### Reduced Decisions

Conventions answer questions before users ask them:

- Where do files go? The directory structure determines placement.
- What should files contain? Standard formats define expectations.
- How do components behave? Category membership dictates behavior.

Each convention eliminates a configuration decision.

### Predictability

Conventions create predictable systems:

- Finding files becomes straightforward because locations follow patterns
- Understanding purpose becomes immediate because names communicate intent
- Navigation becomes intuitive because structure remains consistent

Predictability reduces onboarding time and prevents errors.

### Consistency

Conventions enforce consistency across projects:

- All projects follow the same structure
- All files use the same patterns
- All agents have the same expectations

Consistency enables knowledge transfer between projects and teams.

## Designing Conventions

Effective conventions follow a deliberate process:

1. **Identify common patterns**: Observe what most cases need
2. **Make common cases default**: Zero configuration for typical usage
3. **Support exceptions**: Provide clear override mechanisms
4. **Document the convention**: Make it discoverable

The best conventions feel inevitable—users wonder why alternatives would exist.

## Override Capability

Conventions must include escape hatches. Projects need customization without abandoning the overall structure.

**Pattern**: Reserve `{standard}/` directories for convention-managed content and `{custom}/` directories for project-specific additions.

Examples across frameworks:

| Framework | Convention Directory | Override Directory |
|-----------|---------------------|-------------------|
| Rails | `app/controllers/` | `app/controllers/concerns/` |
| Maven | `src/main/java/` | `src/main/resources/` |
| Next.js | `pages/` (auto-routed) | `lib/` (custom utilities) |
| Kubernetes | Built-in resource types | Custom Resource Definitions |

The override pattern follows consistent structure: standard directories contain managed content; parallel directories allow additions without modifying conventions. The framework manages everything in standard directories while users control custom directories.

## Anti-Patterns

| Anti-Pattern | Convention Alternative |
|--------------|----------------------|
| Configuration file for directory locations | Fixed directory structure |
| Schema definition for file format | Standard frontmatter fields |
| Plugin system for behaviors | Predetermined category behaviors |
| Optional features requiring opt-in | Features enabled by default |

Each anti-pattern forces users to make decisions that conventions could make for them.

## Relationship to KISS

Convention over configuration enables simplicity:

- Fewer configuration decisions produce simpler interfaces
- Predictable structures enable easier navigation
- Standard patterns reduce learning curves

Conventions encode expert decisions so users inherit expertise without acquiring it. The convention designer invests effort once; all users benefit indefinitely.

## Tradeoffs

Conventions impose constraints:

- **Flexibility**: Some valid use cases may not fit the convention
- **Learning curve**: Users must learn the conventions
- **Migration**: Changing conventions affects all adopters

These tradeoffs are acceptable when conventions reflect genuine best practices. Poor conventions impose costs without delivering benefits. Good conventions feel like natural constraints that prevent mistakes.
