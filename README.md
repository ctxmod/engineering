# Engineering

Language-neutral software engineering doctrine for ctxmod.

Profiles:

- `principles` (default): design principles, code organisation, naming, interfaces, errors, testing, and performance.
- `repositories`: repository design — layout, ADRs, tooling, package boundaries and testing tiers — and delivery practice: versioning and commits, trunk-based change flow and review, supply-chain integrity, delivery metrics and error budgets. Extends `principles`.

Select profiles in a consuming module's `requires` or a session selection, e.g. `github.com/ctxmod/engineering/context/profiles/repositories`.
