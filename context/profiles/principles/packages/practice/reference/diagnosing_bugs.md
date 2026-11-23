# Diagnosing Bugs

A discipline for hard bugs and performance regressions. The phases run
in order; skip one only with an explicit justification recorded in the
work notes. Before exploring, read for context: `PROJECT.md` and the
reference corpus of the repository's context module for the areas in
play, plus the decision records covering the area you are touching.

## Redact Before You Show

Diagnosis produces commands, outputs, and captured artifacts that land
in reports and commit messages. Redact every secret first: write
`<REDACTED>` in its place. Build loops against environment variables so
credentials stay out of what you capture, and quote only the lines that
carry the signal.

## Phase 1: Build a Feedback Loop First

The loop is the discipline; everything after it is mechanical. A tight
pass/fail signal that goes red on this specific bug will find the
cause — bisection, hypothesis testing, and instrumentation all consume
it. Spend disproportionate effort here, and refuse to move on without
one: no red-capable command, no hypothesising. Reading code to build a
theory before a red-capable command exists is the exact failure this
discipline prevents.

Construction seams, roughly in preference order:

1. **Failing work test** at whatever seam reaches the bug; work tests
   may import internal packages, so implementation-coupled seams are
   reachable.
2. **Test-tool invocation** of the real binary with scripted input,
   asserting on its data-payload stdout and exit code.
3. **CLI invocation with a fixture**, diffing structured output
   against a known-good snapshot.
4. **Replay a captured trace**: push a saved payload, log, or event
   stream through the code path in isolation.
5. **Throwaway harness**: a minimal standalone program exercising the
   suspect path with one call, built outside the production tree.
6. **Differential loop**: same input through the previous binary and
   the current build; diff the outputs.
7. **Bisection harness**: automate "build at commit X, check, repeat"
   so `git bisect run` can consume it.
8. **Property or fuzz loop**: many generated inputs, watching for the
   failure mode.
9. **Benchmark harness** for performance regressions: measure the
   baseline, then bisect (the perf branch under Phase 4).
10. **Structured human-in-the-loop loop**, last resort: when a human
    must operate the interface, route steps and observations through
    a notification channel or an approval-flow tool so captured
    results feed back to the agent — prose concept, no
    vendored script.

Costed anti-pattern: stage-internal pipeline behavior (record
washing, payload shaping, merge) is invisible to black-box runs, and
build-probe-inspect cycles burn hours on questions a
pipeline-package test fixture settles in seconds.

A loop is done when it is **red-capable** (drives the actual bug path
and asserts the user-visible symptom, so it fails on this bug and
passes once fixed), **deterministic** (same verdict every run), **fast**
(seconds, not minutes), and **agent-runnable** (unattended, with one
command you have already run at least once, its output in hand).

### Tighten the loop

Treat the loop as a product: make it faster (cache setup, narrow the
scope), make the signal sharper (assert the specific symptom, not
"didn't crash"), and make it more deterministic (pin time, seed
randomness, isolate the filesystem, freeze the network).

### Non-deterministic bugs

The goal is a higher reproduction rate, not a clean one. Loop the
trigger, add stress, narrow timing windows. A bug that reproduces half
the time is debuggable; one that reproduces once in a hundred runs is
not — keep raising the rate until it is.

### When no loop is possible

Stop and say so explicitly. List what you tried, then ask for what
would unblock: access to the environment that reproduces it, a
redacted captured artifact, or permission to add temporary
instrumentation. Do not proceed to hypothesise without a loop.

## Phase 2: Reproduce and Minimise

Run the loop and watch it go red on the symptom as described — not a
nearby different failure, which produces a wrong fix. Confirm it
reproduces across runs (or at the raised rate above), and capture the
exact symptom so later phases can verify the fix addresses it.

Then shrink the reproduction to the smallest scenario that still goes
red: cut inputs, callers, config, and steps one at a time, re-running
the loop after each cut, keeping only what is load-bearing. Done means
every remaining element is load-bearing — removing any one turns the
loop green.

## Phase 3: Hypothesise

Write three to five ranked, falsifiable hypotheses before testing any
of them; a single hypothesis anchors on the first plausible idea. Each
must state a prediction: "if X is the cause, changing Y makes the bug
disappear / changing Z makes it worse." A hypothesis without a
prediction is a vibe — sharpen it or discard it.

## Phase 4: Instrument One Variable

Each probe maps to a specific prediction from Phase 3, and each change
alters one variable. Prefer targeted logs at the boundaries that
distinguish hypotheses; never log everything and grep. Tag every debug
log with a unique prefix so cleanup becomes a single search — untagged
logs survive, tagged logs die.

Performance regressions invert this: logs are usually the wrong tool.
Establish a baseline measurement first (a benchmark harness, a
profiler, a query plan), then bisect. Measure first, fix second.

## Phase 5: Fix Behind a Regression Test

Write the regression test before the fix — but only where a correct
seam exists. A correct seam exercises the real bug pattern as it
occurs at the call site; a seam too shallow to replicate the
triggering chain produces false confidence. If no correct seam exists,
that itself is the finding: the architecture is preventing the bug
from being locked down, and it gets flagged as such.

With a correct seam: turn the minimised repro into a failing test,
watch it fail, apply the fix, watch it pass, then re-run the Phase 1
loop against the original un-minimised scenario.

Reachability is half the seam: a tolerance latent for months —
`os.IsNotExist` on a `%w`-wrapped error never matches, so the guard is
dead until the not-found path becomes reachable — reds only once an
unrelated change opens that path:
`ctx:github.com/ctxmod/go/context::shared::pitfalls@core/reference/errors_is_not_exist.md`.

## Phase 6: Clean Up

Before declaring done: the original reproduction no longer reproduces
(re-run the Phase 1 loop); the regression test passes, or the missing
seam is documented; every tagged debug log is removed; throwaway
harnesses are deleted or moved to a clearly marked debug location; and
the commit message states which hypothesis turned out correct, so the
next debugger inherits the answer — failed hypotheses and the winning
one are decision history, carried the way a decision record would be.

Verification seams for these phases — package-level tests and their
execution, and test tools for black-box binary evidence — sit inside
the verification-tier model that separates quality claims
from working-behavior claims: `ctx:github.com/ctxmod/agentic/context::shared::evidence@core/reference/deliberate_verification.md`.
