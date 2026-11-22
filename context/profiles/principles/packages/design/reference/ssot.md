# Single Source of Truth (SSOT)

Every piece of information has exactly one authoritative location. All other uses reference that location.

## Core Principle

When information exists in multiple places, it eventually becomes inconsistent—one location gets updated while others remain stale. SSOT prevents this by design: information lives in one place, all consumers reference that place, and changes propagate automatically.

## Identifying the Source

When multiple files could contain information, determine the correct source by asking:

1. Where would a reader expect to find this information?
2. Which file's purpose most closely matches this content?
3. Which location minimizes the need for updates when the information changes?

The answers usually converge on a single location. If they diverge, the first question takes priority: reader expectations determine discoverability.

## Ownership Hierarchy

Documentation systems benefit from clear ownership patterns:

| Category | Role |
|----------|------|
| Core definitions | Define authoritative truth |
| Policies | Reference truth, add behavioral constraints |
| Procedures | Reference truth, aggregate steps |
| Interfaces | Reference truth, define contracts |

This hierarchy establishes clear direction: derived content references source content, never the reverse. Core definitions contain the knowledge; other categories apply it.

## File-Level Application

Within a single file, state each concept once:

**Problematic**:
```markdown
## Section A
The system must validate input before processing.

## Section B
Remember to validate all inputs before the system processes them.
```

**Correct**:
```markdown
## Input Validation
The system validates input before processing.

## Processing
[References validation as prerequisite without restating it]
```

The second version allows the validation requirement to change in one place.

## Reference Mechanisms

Cross-references, imports, and includes all preserve SSOT by maintaining the source relationship rather than copying content. The reference principle covers these mechanisms comprehensively.

<ctxmod_path path="ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/reference_over_copy.md" />

## Benefits

Single update point (change one file, all references stay current), guaranteed consistency (no conflicting information possible), clear discovery (each information type has an obvious home), and easy verification (one location to confirm accuracy).

## When Duplication Seems Necessary

Extract shared content to a dedicated file, then have all consumers reference it (adjusting verbosity per context). DRY covers the extraction pattern in detail.

<ctxmod_path path="ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md" />

## Acceptable Exceptions

Duplication is acceptable when:

- Content is trivially small (single word or number)
- The source is external and unstable
- Performance requirements demand local copies (rare in documentation)

These exceptions remain rare. Default to SSOT.
