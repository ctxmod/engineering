# ADR Discipline

Architecture decision records are the repository's decision memory. The
discipline exists so a future reader can learn not only *what* was decided
but *what was known, what was rejected, and when the decision changed* —
without reading git archaeology or tribal history.

## Record Shape

Each record is numbered, dated, and immutable in substance:

- **Numbered** — a monotonic identifier (`ADR-0041`) that other documents,
  code comments, and reports cite. Numbers are never reused.
- **Dated** — the decision date is part of the record, not inferred from git.
- **Statused** — proposed, accepted, superseded. The status line is the
  record's live field; everything else is history.

## Append-Only History

Amendments append; they never rewrite. A reversed decision produces a *new*
record that names the record it supersedes, states the trigger, and carries
its own date. The superseded record gains a superseded-by note in place —
pointing forward, its body untouched. Decision history is then readable in
one direction: follow the supersession chain and the reasoning evolves in
order. Rewriting a superseded record to "fix" it destroys exactly the
context the chain exists to preserve.

Dated in-place corrections are reserved for factual repairs that do not
change the decision (a wrong version number, a renamed path), with the
correction date noted — the distinction is substance: facts can be corrected
where they sit; decisions can only be changed by a new record.

## Machine-Readable Projection

Every record is paired with a structured projection (one entry per record in
a machine-readable index: id, date, status, title, supersession links).
The projection and the prose are two views of one source of truth — when
they diverge, that divergence is itself a defect, not a formatting nit:

- a record that exists in prose but not in the projection is invisible to
  tooling that consumes decisions;
- a projection row whose prose counterpart is missing is a phantom decision.

## Guard-Pinned Counts

The mechanical invariants belong to guard tests, not prose: the count of
records matches the projection, every superseded record links to a successor
that exists, every accepted record projects with status `accepted`. Prose
that asserts counts ("we have forty-one ADRs") rots on the next merge; a
guard test fails loudly when the pairing breaks, which is the only behavior
that keeps the two views honest under churn.

## When to Record

A decision earns a record when all three tests hold:

1. **Hard to reverse** — changing the decision later carries meaningful
   cost.
2. **Surprising without context** — a future reader of the code will
   wonder why it was done this way.
3. **The result of a real trade-off** — genuine alternatives existed and
   one was chosen for stated reasons.

A decision failing any test needs no record: reversible decisions get
reversed; unsurprising ones answer themselves; a choice without
alternatives records nothing beyond "we did the obvious thing." What
qualifies: architectural shape, technology choices that carry lock-in,
boundary and scope decisions (the explicit no-s as much as the yes-s),
deliberate deviations from the obvious path, constraints invisible in
the code, and rejections that would otherwise be re-proposed.

## Glossary Discipline

Terminology is part of the decision memory. The project's canonical
vocabulary lives in `PROJECT.md` and the reference corpus of the
repository's context module —
there is no separate glossary artifact — so term discipline applies
wherever that corpus is authored or edited:

- Challenge a term that conflicts with established usage the moment the
  conflict surfaces: which definition governs?
- Propose a precise canonical term when discussion runs on a vague or
  overloaded one, recording the avoided variants beside the chosen one.
- Stress-test domain relationships with concrete scenarios that force
  the boundary between two concepts to be stated.
- Cross-check terminology claims against the code; a contradiction
  between the stated model and observed behavior is a finding, resolved
  in favor of whichever is right.
- Capture a resolved term in the corpus immediately — inline as it
  crystallises, never batched for a later pass.

If a resolved term strains to find a home in `PROJECT.md` or the
corpus, that is a vocabulary-architecture question to surface, not a
reason to invent a new artifact.
