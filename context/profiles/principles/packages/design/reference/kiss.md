# KISS Principle

The highest form of engineering sophistication is making the intricate feel inevitable. Simple interfaces hide sophisticated mechanisms.

## Core Principle

KISS represents the disciplined elimination of everything that does not directly serve the core purpose. This requires deep mastery of underlying complexity—the designer must comprehend the full domain, anticipate failure modes, and encode that knowledge into clean structures.

## Paradox of Simplicity

Creating simple abstractions requires more expertise, not less:

- Understanding the full domain
- Identifying essential vs accidental complexity
- Encoding expert knowledge into maintainable structures
- Reducing cognitive load without reducing capability

Simplicity is the result, not the starting point.

## Essential vs Accidental Complexity

**Essential complexity**: Inherent in the problem domain. Cannot be eliminated without changing the problem.

**Accidental complexity**: Introduced by the solution. Can and should be eliminated.

KISS ruthlessly excises accidental complexity while respecting essential complexity.

## Indicators of Violation

| Symptom | Likely Cause |
|---------|--------------|
| Extensive configuration required | Missing sensible defaults |
| Documentation longer than implementation | Accidental complexity |
| Users need deep knowledge to operate | Insufficient abstraction |
| Frequent edge cases and gotchas | Incomplete modeling |

## Achieving Simplicity

1. **Understand deeply**: Master the domain before designing
2. **Identify patterns**: Find recurring structures and behaviors
3. **Extract abstractions**: Create interfaces that capture patterns
4. **Provide defaults**: Make common cases trivial
5. **Hide mechanisms**: Implementation details stay internal

## Convention Over Configuration

Strong defaults enable simplicity. When 90% of use cases share the same configuration, make that configuration the default. Support customization without requiring it.

## Cognitive Load

Simple systems minimize cognitive load:

- **Recognition over recall**: Obvious next steps
- **Consistent patterns**: Learn once, apply everywhere
- **Clear feedback**: System state is apparent
- **Graceful errors**: Failures guide toward solutions

## Anti-Patterns

| Anti-Pattern | KISS Alternative |
|--------------|------------------|
| Exposing all options | Sensible defaults with override capability |
| Requiring initialization | Auto-initialization with convention |
| Complex dependency graphs | Clear, shallow hierarchies |
| Leaky abstractions | Complete encapsulation |

## Simplicity as Goal

Simplicity is the goal of system design. Every file, pattern, and mechanism should feel inevitable to its users—as if no other design could exist.
