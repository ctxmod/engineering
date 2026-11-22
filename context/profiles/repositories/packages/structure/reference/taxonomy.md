# Directory Taxonomy

A polyglot product repository divides its top level by role, not by language
or team. Each directory answers one question about the things inside it, and
the answer is visible from the path alone.

## The Top-Level Slots

| Directory | Holds | Defining question |
|---|---|---|
| `pkg/<lang>/` | Publishable shared libraries, one subtree per language | "Would another repository import this?" |
| `tools/` | Developer-facing binaries and repo automation | "Does this exist to build, check, or operate the repo?" |
| `apps/` | User-facing deployable products | "Does a person interact with this directly?" |
| `services/` | Long-running deployable backends | "Does another program talk to this over the network?" |
| `infra/` | Provisioning and platform configuration | "Does this define the environment things run in?" |
| `eval/` | Verification of non-deterministic systems | "Is the verdict statistical rather than Boolean?" |
| `tests/` | Deterministic black-box verification programs | "Is the verdict Boolean and repeatable?" |
| `docs/adr/` | Architecture decision records | "Did we decide this?" |
| `context/` | Agent context: the repository's ctxmod module — profiles, rules, references | "Does an agent need this to work here?" |
| work store | Agent work state: items, reports, memories (the ctxmod work store, `ctxmod work …`) | "Is this in-flight or archival work state?" |

Placeholders (`<lang>`, `<repo>`) are deliberate: the slots are the
portable part, the names inside them are per-repository.

## The Load-Bearing Splits

- **Source packages carry no in-package test suites.** Verification is
  external: `tests/` holds black-box programs that exercise built binaries
  against fixtures, and `eval/` holds the statistical suites for the parts
  whose quality is a distribution, not a Boolean. A source tree with
  `_test.go` / `*.test.*` files interleaved is a taxonomy violation, not a
  style preference — it couples verification to implementation and hides the
  test inventory from the directories whose job is to show it.
- **`tests/` versus `eval/` is the determinism line.** A check that must pass
  every run or fail every run belongs in `tests/`. A check whose honest
  output is a score, a rate, or a distribution — model-backed behavior,
  fuzzy extraction, anything with a statistical gate — belongs in `eval/`.
  Forcing an eval into the tests tier produces flaky suites; forcing a test
  into eval hides a determinism bug behind thresholds.
- **`pkg/` is the publish boundary, not the default home.** Code lands in a
  language's `pkg/` subtree when it is meant for reuse across boundaries;
  single-consumer logic lives inside the app, service, or tool that uses it.
  Promoting to `pkg/` is the deliberate act of accepting an API-support
  obligation.

## What the Taxonomy Buys

Tooling scopes by role: path filters in CI, index tags, lint scoping, and
agent context all key off the top-level split instead of per-file
heuristics. The taxonomy is therefore also a contract with the repo's
automation — moving a concern across a slot boundary is a decision about
what the automation will now assume about it.
