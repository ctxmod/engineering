# Package Boundary Discipline

Package boundaries are the repository's real architecture — the file layout
suggests structure, but the import graph enforces it. Two rules carry the
whole discipline: when to cut a boundary, and which direction edges may
point. One blind spot survives every mechanical guard: consumers outside
the repository, whose edges no in-repo scan can see.

## Boundary or Entrypoint?

**A directory is a package when something other than output weight justifies
the boundary** — a distinct API surface, a distinct ownership or review
boundary, a seam kept replaceable, a dependency class kept separate. **It is
an entrypoint when output weight is the only argument** — a binary that must
compile and link separately, an aggregation main.

The failure mode is boundary-by-reflex: one directory per file, one file per
type, until every concept change scatters across ten `package` declarations
and the import graph is a hairball with no readable shape. The opposite
failure — one package for everything — makes dependency direction
unenforceable. The question to ask is not "could this be its own package?"
but "what would this package's importers be forbidden from reaching if I
drew the line here?"

## The Rank Graph

Assign every package a **numeric rank** reflecting its layer: foundation
packages lowest, application composition highest. The import rule is one
sentence:

> A package may import only packages of strictly lower rank.

Consequences that make the graph auditable:

- **Edges are one-directional by rank**, so cycles are structurally
  impossible: an import from rank 3 to rank 3, or 3 to 2, fails the rule
  before it fails the build.
- **The rank table is pinned by a guard test** that walks the import graph
  and fails on any edge that violates the declared rank — the table in the
  test is the SSOT, and a new package without a declared rank fails loudly
  instead of silently inheriting one.
- **Ranks are few.** A five-rank graph is readable; a twenty-rank graph is a
  hairball with extra steps. Most repositories need: foundations, shared
  domain, per-capability packages, composition, entrypoints.

## What Violations Look Like

- **The reach-back import**: a foundation package importing a domain type
  "just for this one helper." The fix is the helper moving up, or the
  dependency moving down — never the edge.
- **The peer sneaking sideways**: two same-rank packages importing each
  other, usually after one grows a convenience function the other wanted.
  Extract the shared part down a rank.
- **The entrypoint that grew a public API**: other code importing from a
  main-package directory. Entrypoints are leaves; anything imported is a
  package, and rank rules apply.

## Guards Beat Review

Boundary rules are mechanically checkable, so they are enforced
mechanically: the rank guard test, import-linter configuration, or the
repository's equivalent. Review catches *why* a boundary should move;
mechanics catch *that* an edge crossed one. A discipline that relies on
reviewers remembering the rank table decays with exactly the pressure that
makes it matter.

## The Out-of-Repo Consumer Blind Spot

The rank graph and its guard test see only the repository's own import
graph; an edge from a consumer outside the repository is invisible to
both. A library repository is rarely import-closed: a sibling repository
can carry a replace-dependency on it and import its library packages
directly — spanning shared helper packages and tool-layer packages, not
just the foundations. Such a census only grows over time: one measured
case counted a handful of directly imported packages, and a re-census a
few weeks later counted 47 distinct packages.

This is the failure mode of the "zero importers means internalize"
heuristic. In the measured case, a cloud-tunnel package showed zero
in-repo importers — the classic internalize-now signal — but the sibling
repository's imports made it a cross-repo public contract, and
internalizing it would have broken the sibling's build outside the
library repository's sight.

**Rule:** before ANY rename, internalization, or signature change in a
library package, census the out-of-repo consumers in addition to the
in-repo census — search every known consumer checkout for the package's
import path:

```sh
rg --no-ignore "<module-path>/<pkg>" <consumer-checkout>/
```

## Cross-References

- <ctxmod_path path="ctx:github.com/ctxmod/engineering/context::shared::structure@repositories/reference/taxonomy.md" /> — the directory taxonomy the rank graph
  refines into import-direction discipline.
