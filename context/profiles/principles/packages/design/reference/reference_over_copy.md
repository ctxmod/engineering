# Reference

Referencing creates connections between artifacts without duplicating content. A reference points to source material, maintains provenance, and enables updates to propagate automatically. The principle applies universally: code, documentation, reports, summaries, discovery, validation, tasks, specifications, configurations, APIs, and data pipelines.

## Core Principle

Copying creates a snapshot detached from its source. Referencing maintains a live connection: consumers always see the current version, changes require updating only the source.

Three universal benefits: efficiency (a reference costs less than the content it points to), consistency (a single source prevents divergence), and traceability (provenance remains explicit for verification against originals).

References also encode relationships. A reference from A to B signals dependency, use, extension, or citation. These explicit relationships enable dependency analysis, integrity checking, and architectural reasoning.

## Benefits

Referencing outperforms duplication in practice. In code, replacing duplicated logic with shared references improves maintainability and reduces the volume of code any change touches. Documentation systems built on canonical sources avoid divergence and update faster, because a change lands in one place. In AI agent contexts, referencing files instead of inlining their content consumes fewer tokens, leaving more of the context budget for reasoning.

## Reference Types

Different domains use different terminology for the same concept—imports, links, citations, dependencies, includes—but all share the same semantic: pointing to another artifact without copying its content.

| Domain | Terminology | Purpose |
|--------|------------|---------|
| Code | import, include, require | Compose behavior from modules |
| Documentation | link, cross-reference | Connect related explanations |
| Reports | citation, source path | Preserve attribution |
| Tasks | depends-on, blocks | Define execution ordering |
| Configuration | extend, inherit, include | Compose settings from layers |
| APIs | schema reference, contract | Enforce consistency across boundaries |
| Discovery | report path, finding reference | Preserve attribution across phases |
| Specifications | reference, import | Compose requirements from standards |
| Data pipelines | source, sink, subscription | Establish data flow |

## Directionality

Forward references point from consumer to source (a report cites its data). Backward references point from source to consumer (a schema lists its endpoints). Most references are forward.

Forward references enable dependency analysis; backward references enable impact assessment. Prefer forward; add backward when navigation benefits justify maintenance cost.

## Anti-Patterns

| Anti-Pattern | Consequence | Solution |
|--------------|-------------|----------|
| Broken references | Dead links, failed resolution | Automated validation |
| Circular references | Infinite loops, initialization deadlocks | Acyclic dependency graphs |
| Orphaned references | Targets deleted, references remain | Impact analysis before deletion |
| Vague references | "See docs" without specific target | Precise file paths or anchors |
| Over-specific references | Coupling to volatile internals | Reference stable abstractions |

## Referencing by Domain

| Domain | Key Principle |
|--------|---------------|
| Code | Import stable abstractions over volatile internals |
| Documentation | Link to canonical sources; never duplicate explanations |
| Reports | Attach source metadata to every finding; pass paths between phases |
| Summaries | Serve as directories pointing to full content, not replacements |
| Validation | Reference schemas and standards; validate against single-source rules |
| Tasks | Explicit dependencies enable parallel execution |
| Discovery | Reference discovery reports; never restate findings in downstream phases |
| Specifications | Reference requirements and standards; compose from canonical sources |
| Configurations | Extend base configs; override specific values per environment |
| APIs | Schema-driven contracts enforce consistency across boundaries |
| Data pipelines | Explicit source and sink references enable flow visualization |

## Reference Density

High reference density increases coupling. Files referencing many volatile targets become brittle. Reference stable foundations frequently, volatile details sparingly.

## Related

SSOT defines the authoritative source, DRY prohibits copying from it, referencing provides the connecting mechanism, and context efficiency motivates the preference for references over inlined content. These principles form a reinforcing cluster.

<ctxmod_path path="ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md" />
<ctxmod_path path="ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md" />
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
