# Change Flow

How a change travels from an editor to production: integrate into one
trunk continuously, in small batches, through a review that guards code
health, into deployable units whose build, configuration and runtime are
kept apart.

## Trunk-Based Development

Everyone integrates into one shared trunk; long-lived branches are the
exception that needs a reason, not the default.

- **Few, short-lived branches.** The research-backed targets DORA
  publishes: three or fewer active branches in a repository, branches
  merged to trunk at least once a day, and no code freezes or separate
  integration phases. Large, late merges are what make freezes
  necessary; small, early merges remove the need.
- **Trunk is always releasable.** The default-tier checks run before a
  change lands, and a red trunk is fixed or reverted before anything
  else lands — never worked around.
- **Incomplete work hides behind a flag, not a branch.** A feature flag
  (or branch by abstraction for large refactors) lets unfinished code
  merge dark. Flags are debt with an owner and a removal date; a flag
  that outlives its rollout is a configuration branch nobody tests.
- **Release branches, if any, are cut from trunk and only receive
  cherry-picked fixes** that land on trunk first; nothing is developed
  on them.

## Small Batches

Batch size is the lever behind both speed and safety: a small change is
quicker to review, cheaper to revert, and easier to bisect. As a working
calibration from published review guidance, around a hundred changed
lines is usually a reasonable change and a thousand is usually too large —
the reviewer, not a hard limit, decides. Split by layer, by refactor
versus behavior change, or by stacking dependent changes; never by
leaving the trunk broken between them.

## Code Review

Review exists to keep the codebase's health improving over time, not to
reach perfection on one change.

- **Approval standard.** Approve once the change definitely improves the
  overall health of the code, even if it is not perfect; there is no
  perfect code, only better code. Non-blocking suggestions are marked as
  such (a "nit:" prefix or equivalent).
- **Latency is part of quality.** Respond to a review request quickly —
  published guidance sets one business day as the maximum — because slow
  review pushes authors toward large batches, which defeats review.
- **Mechanics before people.** Formatting, lint, type checks, boundary
  guards and commit-message checks run before a human looks, so review
  attention goes to design, correctness, naming, tests and
  documentation (see Guards Beat Review in the package-boundaries
  reference).
- **Review the behavior, not only the diff.** A change that removes an
  error branch, an accessibility hook or a test in the same change that
  "fixes" a finding is a behavior change and is reviewed as one (the
  lint-versus-behavior pre-flight in the practice package).
- **Ownership is routed, not remembered**: path-to-owner tables decide
  who must review (the monorepo section of the tooling contract).

## The Deployable-Unit Baseline (Twelve-Factor)

The twelve-factor methodology remains the shortest statement of how a
deployable service relates to its platform: one codebase with many
deploys; explicitly declared and isolated dependencies; config stored in
the environment; backing services as attached resources; strictly
separated build, release and run stages; stateless processes; services
exported by port binding; scale-out through the process model;
disposability (fast start, graceful shutdown); dev/prod parity; logs as
event streams; admin tasks as one-off processes.

Status: the methodology was open-sourced in November 2024 and a
community refresh is under way in its public repository, but the
canonical text is still the 2017 revision — no revised factor set has
been published. Treat the twelve factors as the baseline and the
refresh's discussions (for example the application–platform interface
posts on the project blog) as direction, not yet as doctrine.

## Sources

- https://dora.dev/capabilities/trunk-based-development/ — DORA capability "Trunk-based development": three or fewer active branches, merge at least daily, no code freezes or integration phases; accessed 2026-11-23.
- https://google.github.io/eng-practices/review/reviewer/standard.html — Google engineering practices, "The Standard of Code Review" (approve when the change definitely improves code health); accessed 2026-11-23.
- https://google.github.io/eng-practices/review/reviewer/speed.html — "Speed of Code Reviews" (one business day maximum response); accessed 2026-11-23.
- https://google.github.io/eng-practices/review/developer/small-cls.html — "Small CLs" (≈100 lines reasonable, ≈1000 usually too large); accessed 2026-11-23.
- https://12factor.net/ — The Twelve-Factor App (factor list; page states "Last updated 2017"); accessed 2026-11-23.
- https://12factor.net/blog — maintainers' blog: open-source announcement and refresh (2024-11-12), monthly updates (2024-12-03), later posts through 2025-04-03; accessed 2026-11-23.
