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

## Division with Multi-Language Orchestration

The repo toolchain contract governs individual tools. When a repository
carries several language tiers, each tier's task orchestration is owned by
that tier's own toolchain guidance (the JS/TS orchestrator positioning lives
in the frontend guidance, for example); the tooling contract here is the
interface every tier's tools conform to, regardless of which orchestrator
invokes them.
