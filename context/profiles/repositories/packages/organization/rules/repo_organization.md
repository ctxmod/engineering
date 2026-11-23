# Repository Organization

Directory taxonomy, ADR discipline, tooling contract, package-boundary ranks, and the testing tier split.

A repository's top level divides by role, and its boundaries, decision records, toolchains, and verification tiers each carry their own discipline.

- the directory taxonomy and the tests/eval determinism line — <ctxmod_path path="ctx:github.com/ctxmod/engineering/context::shared::structure@repositories/reference/taxonomy.md" />
- numbered dated ADRs in the Nygard or MADR 4.0.0 format, append-only amendments, and the guard-pinned projection — <ctxmod_path path="ctx:github.com/ctxmod/engineering/context::shared::structure@repositories/reference/adr_discipline.md" />
- the CLI contract repo tooling conforms to (`--check`, structured stdout, loud failures, exit codes, goldens) and its monorepo scaling (affected-target selection, content-addressed caching, ownership tables) — <ctxmod_path path="ctx:github.com/ctxmod/engineering/context::shared::structure@repositories/reference/tooling_contract.md" />
- the boundary-versus-entrypoint rule and the rank-graph import discipline — <ctxmod_path path="ctx:github.com/ctxmod/engineering/context::shared::structure@repositories/reference/package_boundaries.md" />
- the hermetic / on-demand / manual-evidence testing tier split and its mapping to small / medium / large test sizes — <ctxmod_path path="ctx:github.com/ctxmod/engineering/context::shared::structure@repositories/reference/testing_tiers.md" />
