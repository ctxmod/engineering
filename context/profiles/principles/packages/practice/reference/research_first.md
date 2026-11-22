# Research-First Development

Before implementing anything, perform external research to ground decisions in the latest best practices. Favor modern patterns discovered through research over assumptions or legacy patterns.

## Core Principle

Agent knowledge has a cutoff date. Best practices evolve. External research bridges this gap, ensuring implementations reflect current standards rather than outdated assumptions.

## Research Sources

| Source | Purpose |
|--------|---------|
| Academic databases | Foundational research, proven techniques |
| GitHub | Real-world implementations, community patterns |
| Technical blogs | Practical insights, recent developments |
| Documentation | Official guidance, current APIs |
| Forums | Community consensus, known issues |

## Research Workflow

### 1. Identify Knowledge Gaps

Before implementation, identify what you don't know:

- Current best practices for the approach
- Modern alternatives to known patterns
- Recent developments in the domain
- Community consensus on trade-offs

### 2. Conduct Research

Use available search tools:

- Perplexity search for general best practices
- Academic search for foundational research
- GitHub search for implementation patterns
- Web search for recent discussions and updates

### 3. Synthesize Findings

Create a research report documenting:

- Key findings from each source
- Consensus patterns across sources
- Conflicting recommendations and resolution rationale
- Recommended approach with supporting evidence

### 4. Validate Against Codebase

Compare research findings with existing codebase:

- Identify alignment with current patterns
- Identify gaps or outdated patterns
- Determine migration path if needed

## When to Research

Research before:

- Implementing new features
- Defining new patterns or principles
- Making architectural decisions
- Adopting external tools or libraries
- Writing specifications

Skip research for:

- Trivial changes with clear solutions
- Well-established patterns with no recent evolution
- Time-critical fixes (research after stabilizing)

## Research Reports

Document research in structured reports:

```sh
ctxmod work new report "<topic>"   # prints the report file to write
```

Reports persist findings for future reference and reduce redundant research across sessions.

## Legacy Patterns

Legacy code and historical patterns require extra scrutiny. Treat legacy as:

- 10% potentially valuable inspiration
- 90% likely outdated or incorrect

Always validate legacy patterns against current research before adoption.

## Anti-Patterns

| Anti-Pattern | Consequence |
|--------------|-------------|
| Implementing from assumption | Outdated or suboptimal patterns |
| Skipping research for "obvious" solutions | Missing modern alternatives |
| Relying solely on legacy patterns | Perpetuating technical debt |
| Research without synthesis | Information without actionable guidance |

## Primary-Source Discipline

Ground every research claim in the source that owns it. Official
documentation, source code, specifications, and first-party APIs are
primary; summaries, tutorials, and blog write-ups of those sources are
secondary. A secondary source is a lead for where to look — a claim is
grounded only when the primary source confirms it.

- Follow every claim back to its primary source before it enters the
  findings. When a claim cannot be traced, mark it unverified instead of
  letting it pass as established.
- Cite the primary source beside each claim in the write-up, so readers
  re-verify in one hop and a stale claim stays traceable to the text
  that outranks it.
- Distinguish the two write-up shapes: a primary write-up reports what
  the source says (checkable against it); a secondary write-up reports
  an interpretation of the source (judgment, context). Label which one
  the reader is holding.
- Capture findings in-repo, matching the existing convention — research
  reports as report records in the work store (`ctxmod work new report
  "<topic>"`). When no
  convention covers a finding type, choose a sensible location and say
  so in the report, so the next researcher inherits the precedent.
