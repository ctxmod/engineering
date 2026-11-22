# Don't Repeat Yourself (DRY)

Every piece of knowledge must have a single, unambiguous representation in the system. Duplication increases maintenance burden and creates inconsistency risk.

## Core Principle

When information exists in multiple places, changes must propagate to all locations. Some locations inevitably get missed. DRY prevents this by ensuring each piece of knowledge appears exactly once.

## DRY and SSOT

DRY and SSOT are related but distinct principles:

- **SSOT** (Single Source of Truth): Defines where authoritative information lives
- **DRY**: Prohibits copying that information elsewhere

SSOT establishes the source. DRY prevents copies. Together they ensure consistency: one authoritative location, no duplicates.

## Types of Duplication

### Documentation Duplication

The same explanation appears in multiple files with minor variations:

**Problematic**:
```markdown
# config/database.md
Connection pooling improves performance by reusing database connections
instead of creating new ones for each request.

# guides/performance.md
To improve performance, use connection pooling to reuse database
connections rather than creating new ones.
```

**Correct**:
```markdown
# config/database.md
See: docs/concepts/connection-pooling.md

# guides/performance.md
See: docs/concepts/connection-pooling.md
```

Both files now share the same source. Updates propagate automatically.

### Pattern Duplication

The same pattern defined across multiple files:

**Problematic**: Three configuration files each define the same logging format structure.

**Correct**: Structure documented once in a shared module; config files import it.

### Structural Duplication

The same structure repeated with minor variations. When you find yourself copying a structure and making small adjustments, extract the common structure and parameterize the variations.

## Acceptable Repetition

Some repetition does not violate DRY:

- **Coincidental similarity**: Two concepts happen to look alike but represent different ideas
- **Trivial content**: Single words or numbers not worth extracting
- **Deliberate redundancy**: Safety-critical information repeated for emphasis

The key question: "If this changes, must I change it in multiple places?" If yes, it violates DRY.

## Extraction Pattern

When you identify duplication:

1. **Identify the knowledge**: What concept is being duplicated?
2. **Choose a location**: Where should the authoritative version live?
3. **Create the source**: Write the knowledge once in that location
4. **Replace duplicates**: Change all copies to references

## Reference Mechanisms

Use cross-references to avoid copying:

```markdown
See: docs/principles/dry.md
Import: from core.principles import dry_principle
Include: !include ./shared/dry-config.yaml
```

Cross-references create connections without duplication. Imports and includes embed content while maintaining the source relationship. Both mechanisms preserve DRY because the source remains authoritative.

## Benefits

1. **Single update point**: Changes propagate automatically
2. **Guaranteed consistency**: No drift between copies
3. **Reduced maintenance**: Less content to maintain
4. **Clear ownership**: One location owns each concept

## Cost of Duplication

Each duplicate creates:

- **Maintenance overhead**: N copies require N updates
- **Inconsistency risk**: Copies drift over time
- **Confusion**: Readers encounter conflicting versions
- **Wasted effort**: Same work performed multiple times

These costs compound as the system grows. Early extraction prevents accumulating debt.
