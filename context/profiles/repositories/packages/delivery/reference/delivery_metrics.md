# Delivery Metrics and Error Budgets

Two instruments tell a team whether its change flow is healthy: delivery
metrics measure how fast and how safely changes reach users, and an error
budget turns a reliability target into a rule for when to slow down.
Both are measured per application or service, and both decay into
theatre the moment they become targets in their own right.

## The Five DORA Metrics

DORA's software delivery performance model (definitions as published in
the DORA metrics guide, last updated 2026-01-05) has five metrics in two
groups:

| Group | Metric | Definition |
|---|---|---|
| Throughput | Change lead time | time from a change being committed to version control to it being deployed in production |
| Throughput | Deployment frequency | number of deployments over a period, or time between deployments |
| Throughput | Failed deployment recovery time | time to recover from a deployment that fails and requires immediate intervention |
| Instability | Change fail rate | ratio of deployments that require immediate intervention following the deployment |
| Instability | Deployment rework rate | ratio of deployments that are unplanned and happen as a result of an incident in production |

Changes from the older "four keys" vocabulary still common in tools and
write-ups: *time to restore service / MTTR* was renamed **failed
deployment recovery time** in 2023 and narrowed to failures caused by
production changes; in 2024 **deployment rework rate** was added as the
fifth metric and recovery time moved from the stability group to
throughput — it measures how fast a fix flows, not how often breakage
happens. Reliability (availability), sometimes miscounted as a fifth
metric since 2021, is an operational measure outside the five.

Using them well:

- **One service at a time.** The metrics are meant for an application or
  service in its own context; comparing a mainframe batch system with a
  mobile app, or ranking teams against each other, is the documented
  pitfall.
- **Track trends, not thresholds.** Improvement over time for the same
  service is the signal; DORA notes that top performers do well on all
  five at once, so speed bought with instability shows up in the second
  group.
- **Goodhart's law applies.** A mandate such as "every service deploys
  daily by year end" invites gaming; use the metrics to find the
  constraint, then work on the capability (trunk-based development,
  small batches, test automation) that moves it.
- **Measure from systems of record** — version control, the deploy
  pipeline, the incident tracker — so the numbers are a by-product of
  the change flow rather than a survey of it.

Report context: DORA's 2025 report ("State of AI-assisted Software
Development") presents seven team archetypes that combine delivery
performance with well-being and friction, and a DORA AI Capabilities
Model of practices that amplify AI's effect. Neither changes the five
metric definitions above.

## Error Budgets

An SLO (for example 99.9% of requests succeed over 28 days) implies an
error budget: the 0.1% of unreliability the service may spend. The budget
is only useful with a written **error budget policy**, agreed in advance
between the people who ship features and the people who run the service:

- **While within budget**, releases proceed under the normal policy —
  the budget is permission to take risk, and an untouched budget is a
  sign the SLO is stricter than users need.
- **When the budget is exhausted** for the window, the published example
  policy halts all changes and releases except urgent fixes and security
  fixes until the service is back within its SLO; reliability work takes
  priority over features.
- **Large single burns trigger learning**: in the same example, one
  incident consuming more than 20% of the four-week budget requires a
  postmortem with tracked action items.
- **Escalation is part of the policy** — who decides when the parties
  disagree is written down before the disagreement.

Error budgets and the instability metrics answer the same question from
two sides: change fail rate and rework rate say how often deployments
hurt; the budget says how much hurt users have absorbed and therefore
whether the next deployment should wait.

## Sources

- https://dora.dev/guides/dora-metrics/ — DORA metrics guide: five metrics, throughput/instability grouping, application-level use and pitfalls (last updated 2026-01-05); accessed 2026-11-23.
- https://dora.dev/guides/dora-metrics/history/ — DORA metrics history (2023 rename to failed deployment recovery time; 2024 rework rate and regrouping; reliability outside the five); accessed 2026-11-23.
- https://cloud.google.com/devops/state-of-devops — 2025 DORA "State of AI-assisted Software Development" report (seven team archetypes, AI Capabilities Model); accessed 2026-11-23.
- https://sre.google/workbook/error-budget-policy/ — *The Site Reliability Workbook*, "Example Error Budget Policy" (2018): release freeze when the four-week budget is exceeded, postmortem when one incident burns more than 20%; accessed 2026-11-23.
- https://sre.google/workbook/implementing-slos/ — *The Site Reliability Workbook*, "Implementing SLOs" (SLOs, error budgets, policy agreement); accessed 2026-11-23.
