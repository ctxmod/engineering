# Lint-vs-Behavior Pre-Flight

How to commit to a UI pattern without colliding with a linter rule, what
a lint fix may touch, and what the sanctioned escape is when they are
irreconcilable. The worked conflict is the 2026-10-06 accessibility
incident: the release was blocked by one remaining finding, and the
session ended in a user ruling that rebuilt the UI region rather than
suppress the rule.

## The pre-flight probe

Before committing to a UI pattern that a linter rule may flag, probe the
rule — at the pinned version, before the pattern is built:

- **Options surface.** Read the rule's options schema. A schema ending
  in `additionalProperties: false` with no relevant properties means
  zero configurability: every future collision is unfixable within the
  rule, and the only outcomes are rework, an authorized override, or a
  ruling. (Biome 2.5.15's `noNoninteractiveTabindex` is the worked
  case — zero options.)
- **Honest-semantics variant.** Before building the pattern, check
  whether an honest-semantics variant passes the rule as shipped. A
  scrollable region that is genuinely labelled and focusable is correct
  accessibility practice; if the rule rejects the honest variant, that
  is a rule-vs-practice conflict to resolve before the component
  exists, not after the release fails.
- **Where the probe lives.** Record the optionality matrix next to the
  linter's configuration (the prevention wording from the incident:
  "pre-flight version-pinned rule audit when a linter is adopted"), so
  the next pattern choice starts from facts instead of a release-time
  discovery.

The cost asymmetry is the point: the probe costs minutes at authoring
time; the collision cost a full UI rework (feed semantics plus test
adaptation) inserted between a failed release and its retry.

## Lint fixes never remove behavior

A lint fix may restructure code; it may never remove behavior. Deleting
a focusable scroll target, an error state, or an accessibility hook to
make a finding count reach zero converts a lint finding into a behavior
regression — and a green gate now certifies the regression. The incident
names the shortcut directly: under a "zero findings" mandate the
writer's cheapest route was deleting the focusable target, and the
coordinator rejected it — "Removing focusable scroll target is not
acceptable. Use supported labelled focusable scroll region/native
keyboard target (honest semantics) and actual browser proof, no
suppression." Review discipline follows: a diff that removes an
interactive element, an ARIA attribute, or an error branch in the same
change that fixes the lint finding is treated as a behavior change,
flagged and re-planned — not accepted as the fix.

## The escape ladder

When an honest pattern and a rule are irreconcilable, the options in
order:

1. **Rework to honest semantics the rule accepts.** The 2026-10-06
   ruling chose this: rebuild the scrollable region as an ARIA feed
   with roving tabindex — "Do it properly, no shortcuts" — with the
   rule left on, no configuration override, no CI tolerance, no
   suppressions.
2. **Authorized scoped override with a naming comment.** When no honest
   variant exists and rework is out of scope, the sanctioned escape is
   an override scoped to the narrowest selector that silences exactly
   the one rule at the one location, carrying a comment that names what
   is overridden and why (the incident's "scoped override, recommended"
   branch). An override without the naming comment is indistinguishable
   from a silent suppression and is not sanctioned.
3. **Escalate for a ruling.** Fake interactive roles, tabIndex
   workarounds, and dishonest semantics are rejected escapes — the
   incident ruled them out explicitly. If neither 1 nor 2 is available,
   the conflict escalates; it is never dissolved by making the code
   lie.

## Worked conflict (2026-10-06)

The sequence, compressed: release blocked on one accessibility finding;
every honest labelled focusable-region variant failed the same rule; the
rule's options schema proved to have zero configurable exceptions; the
escapes (tabIndex=-1, fake interactive role) were rejected as dishonest;
the user ruling ordered the ARIA-feed rebuild with roving tabindex, rule
unviolated. The doctrine above is that sequence with the cost moved to
authoring time: the probe would have shown zero configurability before
the pattern was chosen, and the rework would never have been needed.
