# Versioning and Commit Conventions

A version number is a promise about compatibility, a commit message is a
record of intent, and a changelog is the human-readable join of the two.
Each has a published specification; follow it rather than a house
dialect, so that downstream tooling and readers need no translation.

## Semantic Versioning 2.0.0

SemVer 2.0.0 is the current specification (no later version has been
published). Its contract, in the parts that most often go wrong:

- **Declare the public API first.** SemVer is meaningless without a
  stated public API — documentation, an exported surface, a schema.
  What is not declared public carries no compatibility promise.
- **MAJOR.MINOR.PATCH.** Incompatible API change → MAJOR; backward-
  compatible functionality → MINOR; backward-compatible bug fix → PATCH.
  Deprecating functionality is a MINOR change; removing it is MAJOR.
- **Released versions are immutable.** A published version's contents
  never change; a mistake ships as a new version.
- **`0.y.z` is initial development.** Anything may change at any time;
  the API is not stable. Leaving `0.x` is the act of committing to the
  public API, not a marketing milestone.
- **Pre-release and build metadata.** `1.4.0-rc.1` sorts *before*
  `1.4.0`; `+build.7` metadata is ignored for precedence. Do not encode
  compatibility in build metadata.

What counts as "the API" is broader than function signatures for tools:
CLI flags, exit codes, structured-output schemas, config keys and file
formats are all public surface (the tooling contract's exit-code table
and structured stdout are versioned API).

## Conventional Commits 1.0.0

The commit-message convention that makes the version bump derivable from
history. The message shape is `<type>[optional scope][!]: <description>`,
with an optional body and git-trailer-style footers.

| Signal | Meaning | SemVer correlation |
|---|---|---|
| `fix:` | patches a bug | PATCH |
| `feat:` | introduces a feature | MINOR |
| `!` after type/scope, or a `BREAKING CHANGE:` footer | breaking API change (any type) | MAJOR |
| other types (`docs:`, `refactor:`, `perf:`, `test:`, `build:`, `ci:`, `chore:` …) | allowed; no implied bump | none |

Practice that keeps the convention honest:

- **One logical change per commit**, so a single type is true of the
  whole diff; a commit that is half `feat` and half `refactor` makes
  the derived version wrong in one direction or the other.
- **Breaking changes are flagged where they happen**, not reconstructed
  at release time; the footer carries the migration note the changelog
  will need.
- **Scopes name the repository's own units** (a package, a tool, a
  profile), so history filters by the same boundaries the taxonomy
  draws.
- **Enforce mechanically**: a commit-message check in the same tooling
  contract as every other check (runnable locally, `--check` in CI),
  not a reviewer's memory.

## Changelogs

A changelog is written for humans; it is not a dump of commit subjects.
The Keep a Changelog 1.1.0 shape is the common reference: newest release
first, an `Unreleased` section at the top, one section per version with
its release date, and changes grouped as Added, Changed, Deprecated,
Removed, Fixed, Security. Conventional Commits make a *draft* of that
document generatable; an editor still writes the entries a reader needs
(the migration note for a breaking change, the user-visible effect of a
fix).

## Sources

- https://semver.org/ — Semantic Versioning 2.0.0 (current specification; items on public API, immutability, `0.y.z`, pre-release and build-metadata precedence); accessed 2026-11-23.
- https://www.conventionalcommits.org/en/v1.0.0/ — Conventional Commits 1.0.0 (message structure, `fix`/`feat`/`BREAKING CHANGE` correlation with SemVer, additional types); accessed 2026-11-23.
- https://keepachangelog.com/en/1.1.0/ — Keep a Changelog 1.1.0 (Unreleased section, change categories); accessed 2026-11-23.
