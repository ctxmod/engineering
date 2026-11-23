# Repository Tooling Contract

A repository's own toolchain — generators, config managers, checkers,
scaffolders — is software with users, and its users are both humans and
agents. The tooling contract is the behavioral interface every such tool
shares, so that anyone (and anything) that has learned one tool has learned
them all.

## The CLI Contract

- **`--check` mode: regenerate, compare, exit nonzero on drift.** Every tool
  that produces or rewrites files offers a check mode that regenerates into
  memory (or a temporary location), byte-compares against the tree, and
  exits nonzero naming the drifted files. CI runs `--check`; local runs
  apply. The asymmetry is the point — CI proves the tree is exactly what the
  tool would produce; the developer asks the tool to produce it.
- **Structured stdout mode.** `--stdout-format json` (or the tool's
  equivalent) emits machine-parseable results on stdout; diagnostics and
  progress go to stderr. Stdout is a data payload — a human format and a
  structured format must carry identical information, and scripts consume
  only the structured one.
- **Loud failures name the field.** A validation failure says which file,
  which field, what was expected, and (where the schema allows) the line and
  column. "Invalid config" is a bug in the error message, not a message.
- **Documented exit codes.** A stable table — conventionally `0` success,
  `1` verification failure, `2` usage error — that callers can branch on.
  Callers distinguish "the check failed" from "I invoked it wrong" without
  parsing output.
- **Golden, byte-pinned fixtures.** Tools whose output is consumed
  downstream or committed pin representative outputs byte-for-byte in
  testdata; a change that shifts a byte fails visibly, and fixture diffs
  double as the honest changelog of output-format changes.
- **Everything CI does is runnable locally.** Any CI step that checks or
  produces content invokes the same tool with the same flags a developer
  runs. A check that exists only in CI cannot be reproduced at debugging
  time and drifts from local practice until it gates nothing anyone wrote.

## Subprocess Wrapping for External Tools

External compilers, linters, and formatters are invoked as subprocesses by
the repo tooling — resolved, executed, and their structured output captured —
never reimplemented and never executed through ad-hoc shell fragments
scattered through scripts. One wrapping layer per external tool gives one
place that knows its flags, its output shape, and its version pinning.

## Implementation Language

Prefer a typed, compiled language for repo tooling: single static binaries
start fast, install without runtime bootstrapping, and share typed logic
with the repository's own packages. Go is the usual choice in
Go-carrying repositories for exactly this reason — the tool imports the same
schema types the product code uses, so the tool cannot drift from the
schema.

## Scaling the Contract in a Monorepo

In a large repository the contract only stays cheap when tools stop doing
whole-tree work on every invocation:

- **Affected-target selection.** A check derives what a change can affect
  from the dependency graph (build graph, package graph, path-to-owner
  map) and runs only that, with a documented escape to "everything" for
  releases and for graph-defining files (lockfiles, toolchain pins, root
  config) whose change invalidates the selection itself.
- **Content-addressed caching.** A step whose declared inputs (sources,
  tool versions, flags, environment) hash identically to a previous run
  may reuse that run's outputs, locally or from a shared remote cache.
  The cache is only as sound as the input declaration: an undeclared
  input — an ambient environment variable, the current time, an
  unpinned tool — turns a cache hit into a stale result. Hermetic steps
  are a precondition for sharing a cache, not a nicety.
- **Ownership as data.** Review routing comes from an ownership file
  that maps paths to owning teams (a `CODEOWNERS`-style table), so the
  taxonomy's slots and the review boundary are the same lines.
- **Same selection locally and in CI.** "Everything CI does is runnable
  locally" extends to the selection: the developer's "what does my change
  affect?" query and CI's must return the same set.

## Division with Multi-Language Orchestration

The repo toolchain contract governs individual tools. When a repository
carries several language tiers, each tier's task orchestration is owned by
that tier's own toolchain guidance (the JS/TS orchestrator positioning lives
in the frontend guidance, for example); the tooling contract here is the
interface every tier's tools conform to, regardless of which orchestrator
invokes them.

## Sources

- https://bazel.build/remote/caching — Bazel documentation, "Remote Caching": action-keyed, content-addressed output reuse and the requirement that actions be hermetic and fully declared (one widely documented instance of the pattern); accessed 2026-11-23.
- https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners — "About code owners": path-to-owner tables driving review requests (one host's instance of ownership-as-data); accessed 2026-11-23.
