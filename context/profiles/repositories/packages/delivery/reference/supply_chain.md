# Supply-Chain Integrity

A consumer of an artifact should be able to answer four questions without
trusting the producer's word: *what source was it built from, how was it
built, what is inside it, and who vouches for it?* Provenance, SBOMs and
signatures answer one question each; posture checks measure whether the
project's own practice keeps those answers trustworthy. Name the
specification and version a claim is made against — "SLSA compliant"
without a track, level and spec version is not a claim.

## Provenance: SLSA v1.2

SLSA v1.2 (approved 2025-11-24, backward compatible with v1.1) organises
requirements into tracks with independent levels:

| Track | Levels | What rising levels buy |
|---|---|---|
| Build | L0 none · L1 provenance exists · L2 hosted build platform (signed provenance) · L3 hardened builds | L1 documents how the artifact was built; L2 prevents tampering after the build; L3 prevents tampering during it (isolated builds, signing material out of reach of build steps) |
| Source | L1 version controlled · L2 change history preserved and source provenance generated · L3 organizational technical controls enforced · L4 code review required | from "the source is in version control" to "every change was reviewed, under controls a consumer can verify" |

Build Environment and Dependency tracks are in development and not yet
part of the approved spec. Practice that follows:

- **Generate provenance in the build platform, not in the build
  script.** Provenance the build steps can write is provenance the build
  steps can forge.
- **Verify, don't just emit.** A consumer (deploy step, package
  install) checks the provenance's builder identity and source
  repository against an expected policy; unverified provenance is
  decoration.
- **Source-track controls are repository settings** — protected trunk,
  required review, preserved history — and belong in the same
  configuration-not-convention bucket as the testing tiers.

## Contents: SBOMs

A software bill of materials lists an artifact's components and their
relationships. Two formats dominate; produce one, at build time, from the
resolved dependency graph rather than from a manifest guess:

- **SPDX 3.0.1** — the current SPDX release (3.0 introduced a profile-
  based model: core, software, security, build, AI, dataset and others).
  The ISO/IEC 5962:2021 international standard corresponds to the older
  SPDX 2.2.1; check which one a consumer requires.
- **CycloneDX 1.7** — released 2025-10-21 by OWASP and standardised
  through Ecma TC54 as ECMA-424; beyond components it models services,
  vulnerabilities (VEX), cryptographic assets and attestations.

Ship the SBOM with the artifact it describes (attached, or as a signed
attestation), regenerate it on every build, and keep it scoped to the
artifact — a repository-wide SBOM describes no shipped thing.

## Vouching: Signatures and Transparency

Sign release artifacts and attestations, and prefer signing schemes that
bind the signature to a workload or person identity and record it in a
public append-only transparency log (the Sigstore model: short-lived
certificates issued against an identity token, logged so that signing
events are auditable after the fact). Long-lived signing keys stored as
CI secrets are the weakest link this model removes. Verification policy
names the expected *identity* (which repository's which workflow), not
merely "has a valid signature".

## Posture: Scorecard and the OSPS Baseline

- **OpenSSF Scorecard** scores a repository's practice with automated
  checks: Binary-Artifacts, Branch-Protection, CI-Tests,
  CII-Best-Practices, Code-Review, Contributors, Dangerous-Workflow,
  Dependency-Update-Tool, Fuzzing, License, Maintained, Packaging,
  Pinned-Dependencies, SAST, SBOM, Security-Policy, Signed-Releases,
  Token-Permissions, Vulnerabilities, Webhooks. Use it as a probe for
  regressions and for evaluating dependencies, not as a target to
  game — a check satisfied by configuration nobody exercises is a
  green coat on a skip.
- **OSPS Baseline** (Open Source Project Security Baseline, OpenSSF
  Security Baseline SIG) is a versioned set of MUST-only controls
  organised by maturity level and category; current version
  v2026.08.28. Claim conformance against a named version, as the
  Baseline itself requires.

The controls these instruments share are the durable core: pinned and
automatically updated dependencies, least-privilege CI tokens, no
untrusted input interpolated into privileged workflow steps, protected
trunk with required review, a published security policy, signed
releases, and no committed binaries.

## Sources

- https://slsa.dev/spec/v1.2/ and https://slsa.dev/blog/2025/11/announce-slsa-v1.2 — SLSA v1.2 (approved; announced 2025-11-24; Source Track introduced; Build Environment and Dependency tracks in development); accessed 2026-11-23.
- https://slsa.dev/spec/v1.2/build-track-basics — Build L0–L3 definitions; accessed 2026-11-23.
- https://slsa.dev/spec/v1.2/source-requirements — Source L1–L4 definitions; accessed 2026-11-23.
- https://spdx.github.io/spdx-spec/latest/ — SPDX specification, latest resolves to v3.0.1; https://spdx.dev/use/specifications/ — ISO/IEC 5962:2021 status; accessed 2026-11-23.
- https://cyclonedx.org/specification/overview/ — CycloneDX 1.7 (release date 2025-10-21; ECMA-424 published 2025-12-10); accessed 2026-11-23.
- https://docs.sigstore.dev/about/overview/ — Sigstore overview (identity-bound short-lived certificates, transparency log); accessed 2026-11-23.
- https://github.com/ossf/scorecard/blob/main/docs/checks.md — OpenSSF Scorecard check list; accessed 2026-11-23.
- https://baseline.openssf.org/ — OSPS Baseline, current version v2026.08.28 (previous v2026.02.19, v2025.10.10, v2025.02.25); accessed 2026-11-23.
