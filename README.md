# Engineering

Language-neutral software engineering doctrine for ctxmod.

Profiles:

- `principles` (default): design principles, code organisation, naming, interfaces, errors, testing, and performance.
- `repositories`: repository design — layout, documentation, tooling, CI, dependencies, and versioning. Extends `principles`.

Select profiles in a consuming module's `requires` or a session selection, e.g. `github.com/ctxmod/engineering/context/profiles/repositories`.
