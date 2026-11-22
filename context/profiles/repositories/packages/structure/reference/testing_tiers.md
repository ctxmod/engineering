# Testing Tier Split

Verification effort is not uniform: most checks must be fast and hermetic, a
few are heavy and run on demand, and some are run by a human, once, for
evidence. Cramming all three into one configuration gives a suite too slow
to run often and too noisy to trust. The split is a deliberate
three-configuration model.

## Tier 1 — Hermetic Default

Everything a developer, agent, or CI gate runs by default:

- deterministic, fully fenced — no network, no shared machine state, no
  wall-clock sensitivity, controlled working directories and environment;
- fast enough to run on every change without deliberation;
- exhaustive at the unit and black-box integration level for deterministic
  behavior.

This tier is the default config of the verification tooling — nobody opts
into it, so its cost is the only cost anyone pays routinely. Every feature
that lands lands with coverage here.

## Tier 2 — Registered On-Demand

The heavy suites: end-to-end flows against real runtimes, chaos and
fault-injection scenarios, cross-service contract runs. Two rules
distinguish this tier from an ungated pile:

- **Registration is explicit.** A tier-2 suite must be registered in a
  documented inventory (a config table, a manifest) to be selectable. The
  inventory is what makes "what heavy checks exist?" answerable and what
  keeps an orphaned suite from surviving unrun and forgotten.
- **Selection is deliberate.** The tier runs when the change warrants it —
  before a release, after a reliability change — invoked by an explicit
  selector, never bolted onto the default path.

## Tier 3 — Manual Runs with an Evidence Ledger

Checks that cannot self-certify: long-horizon soak tests, destructive
environment drills, evaluations needing human interpretation. Running one
appends a row to an **evidence ledger** — what ran, against what, when, by
whom, outcome and artifacts. The ledger row is the deliverable: an unrun
claim of "we tested this manually" is not evidence, and a run without its
row did not, for the repository's purposes, happen.

## Cross-Tier Rules

- **The split is configuration, not convention.** Tier membership is
  declared in the verification tooling's config (build tags, tier names,
  registration entries) so selection is mechanical; a discipline that lives
  in documentation alone decays into "everything in the default suite."
- **Nothing runs automatically.** Default-tier execution rides the deliberate
  verification command; heavier tiers require their explicit selector; no
  hook or CI path invents executions the operator did not ask for.
- **A test's tier is about cost and determinism, not importance.** A
  critical check that is cheap and deterministic belongs in tier 1; an
  unimportant check that needs a cluster still does not belong in the
  default path.
