<div align="center">

# SOVA Research

**Portable, verifiable evidence for AI-agent security findings.**

How a security finding about an AI agent can be packaged so that someone else<br/>
can check it, reproduce it, and see exactly where the evidence stops.

<p>
  <img src="https://img.shields.io/badge/PREPRINT-v1.0-FFB000?style=for-the-badge" alt="Preprint v1.0" />
  <img src="https://img.shields.io/badge/PEER%20REVIEW-NOT%20YET-D22128?style=for-the-badge" alt="Not peer reviewed" />
  <img src="https://img.shields.io/badge/PAPERS-CC%20BY%204.0-00B8D9?style=for-the-badge" alt="CC BY 4.0" />
  <a href="https://github.com/TRUSCOR/SOVA-OSS"><img src="https://img.shields.io/badge/CODE-TRUSCOR%2FSOVA--OSS-0B8793?style=for-the-badge" alt="Implementation" /></a>
</p>

Gautam R. Patil · [TRUSCOR Private Limited](https://truscor.org) · gautam@truscor.org

</div>

---

This repository holds the papers. The implementation lives in
**[SOVA OSS](https://github.com/TRUSCOR/SOVA-OSS)**.

---

## Read this first

AI-agent security findings are usually exchanged as prose reports, screenshots,
logs, or replay scripts. Those artifacts preserve useful observations while
staying ambiguous about three things that decide whether a finding is real:

- **what behaviour** is actually being claimed;
- **which parts of the execution context** must stay equivalent for the claim to hold;
- **whether** capture, conversion, or redaction removed the evidence the claim depends on.

The research programme below attacks that problem from several directions. Its
core output is the `.sova` capsule and the `.sova-trace` evidence record.

---

## The preprint

### [`.sova`: Portable, Evidence-Bounded Reproduction of AI-Agent Security Findings](preprints/sova-portable-evidence-reproduction-v1.0.pdf)

**Version 1.0 · 10 pages · CC BY 4.0 · [PDF](preprints/sova-portable-evidence-reproduction-v1.0.pdf) · [Markdown](preprints/sova-portable-evidence-reproduction-v1.0.md)**

Introduces `.sova`, an inert-by-default behaviour capsule, and `.sova-trace`, its
canonical observable evidence stream. The decision procedure composes a typed
outcome contract, an execution-context equivalence envelope, claim-conditioned
evidence requirements, integrity and trust checks, multi-valued reproduction
states, and recomputable repeated-trial summaries.

It separates three operations that are routinely conflated:

| | |
|---|---|
| **Trace playback** | inert inspection of an already recorded timeline |
| **Controlled re-execution** | a fresh, separately authorized run emitting new evidence |
| **Semantic reproduction** | whether the declared outcome recurred in a sufficiently equivalent context |

**Results.** On two frozen same-team corpora the complete mechanism matched all
144 semantic-reproduction labels and all 120 evidence-fidelity labels, where
structural, integrity-only, and profile-only baselines could not represent the
same decision space. A frozen public-contract study matched the expected decision
for 12 contracts across 96 candidate traces while accepting zero incomplete
traces as reproduced. On 11 retrospective cross-model pairs derived from 22
AgentRewardBench WebArena trajectories it returned 2 equivalent, 5 divergent, and
4 inconclusive decisions — a calibrated semantic-text baseline matched all 7
conclusive labels but was forced to collapse the 4 inconclusive cases into a
binary outcome.

**This is a preprint. It has not been peer reviewed.** The evidence is bounded and
largely same-team. It does not claim independent validation, cross-runtime
superiority, field performance against professional tools or testers, access to
hidden model reasoning, recorder honesty, or non-repudiation. Negative and adverse
results are retained in the manuscript rather than removed. The confirmatory gate —
independently authored claim contracts, blinded labels, two independently operated
runtimes, and external security and privacy review — is **not** complete.

---

## Working drafts

Everything below is **an early draft, openly published for transparency about
work in progress.** None of it is peer reviewed, independently validated, or
submitted anywhere. Methods, results, and claims will change. Please don't cite
these as settled results — they are shared so the direction of the work is legible,
not because the work is finished.

| | Paper | What it asks | Stage |
|---|---|---|---|
| **P1** | [Evidence-Guided Trigger-Interaction Search for Stateful AI Agents](drafts/P1-evidence-guided-trigger-interaction-search.md) · [PDF](drafts/P1-evidence-guided-trigger-interaction-search.pdf) | Can evidence from earlier interactions guide the search for the conditions that trigger an agent failure? | Draft · internal benchmark only |
| **P2** | [From Scanner Disagreement to Bounded Evidence](drafts/P2-scanner-disagreement-adjudication.md) | When scanners disagree about a finding, can execution adjudicate between them? | Early draft · incomplete |
| **P4** | [Evidence-Bounded Semantic Reproduction Across Runtimes](drafts/P4-semantic-reproduction.md) · [PDF](drafts/P4-semantic-reproduction.pdf) | Did the same observable behaviour recur, despite allowed stochastic variation? | Draft · superseded in part by the preprint |
| **P5** | [Claim-Conditioned Evidence Fidelity for Behaviour Records](drafts/P5-trace-evidence-fidelity.md) · [PDF](drafts/P5-trace-evidence-fidelity.pdf) | Does the retained evidence still support the claim after capture, conversion, and redaction? | Draft · superseded in part by the preprint |
| **P6** | [Evidence-Bounded Semantic Conformance Across Heterogeneous Executors](drafts/P6-executor-conformance.md) · [PDF](drafts/P6-executor-conformance.pdf) | Do two different agent executors actually agree on what happened? | Draft · internal corpus only |
| **P7** | [Evidence-Bounded Causal Composition Forensics](drafts/P7-causal-composition-forensics.md) | In a partially observed multi-step failure, which steps were actually responsible? | Early draft · mechanism in progress |

Each draft keeps its own limitations and gate-decision section. Those sections are
internal research notes, retained rather than polished away, and they are the
honest record of what is and is not established.

Two further questions — substitution-fidelity transfer, and reducing false
security labels across three axes — are at the question stage with no draft yet.

---

## How this relates to SOVA OSS

[SOVA OSS](https://github.com/TRUSCOR/SOVA-OSS) is the
open-source implementation: the `.sova` and `.sova-trace` formats, schemas, the
reference verifier, examples, and tests. It is pre-alpha and under active development.

The papers specify and test the ideas. The repository is where they are built and
where you can check them. Retained result manifests, preregistrations, build
provenance, and reviewer packets are held by TRUSCOR Private Limited and are not
part of this paper-only release. Third-party artifacts whose redistribution rights
are unclear are not redistributed here.

---

## Licence

Papers in this repository are released under
**[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)** — © 2026 Gautam R. Patil.
You may share and adapt them with attribution.

The SOVA OSS source code is separately licensed under Apache-2.0.

## Citing the preprint

```bibtex
@misc{patil2026sova,
  title        = {{.sova}: Portable, Evidence-Bounded Reproduction of
                  {AI}-Agent Security Findings},
  author       = {Patil, Gautam R.},
  year         = {2026},
  note         = {Preprint, version 1.0. Not peer reviewed.},
  howpublished = {\url{https://github.com/Gautam-R-Patil/sova-research}},
}
```

## AI-assistance disclosure

Gautam R. Patil conceived the research programme, specified the direction and
safety constraints, and directed the work. Implementation, experiment execution,
analysis support, literature discovery, and drafting used AI coding assistants
under his direction. He is the sole human author and reviewed and approved the
text, citations, figures, data, and declarations. No AI system is listed as an
author.

## Contact

Corrections, disagreements, and independent replication attempts are all welcome —
particularly attempts to falsify the results. Open an issue, or write to
gautam@truscor.org.
