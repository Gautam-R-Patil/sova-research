# From Scanner Disagreement to Bounded Evidence

## Execution-Based Adjudication for AI-Agent Security Findings

**Early working draft — not peer reviewed, not independently validated.** Published openly for transparency about research in progress; methods, results, and claims may change.

- Version: 0.3, 2026-08-03
- Proposed human author: Gautam R. Patil
- Affiliation: TRUSCOR Private Limited
- Contact: gautam@truscor.org
- Status: `GO-PROSPECTUS / GO-EXPERIMENT / HOLD-MANUSCRIPT / HOLD-SUBMISSION`

## Abstract

Security scanners for AI agents, skills, Model Context Protocol servers, and
tool-using applications use different target models, rules, judges, and
observation surfaces. Their findings therefore cannot be treated as independent
votes, and agreement does not establish exploitability. We specify a study that
normalizes scanner outputs into claim identities, records shared detection
mechanisms, and subjects eligible disagreements to separately authorized,
execution-based observations. The adjudication output is deliberately bounded:
`confirmed-positive`, `false-positive-under-declared-test`,
`not-observed-under-declared-test`, or `inconclusive`. Every decision retains
the target, environment, authorization, procedure, evidence, uncertainty, and
limitations in a `.sova` capsule and `.sova-trace` evidence stream.

The public SOVA reference implementation supports the interchange and decision
contract, but no representative scanner-disagreement dataset has been run.
Consequently, this paper currently contributes a falsifiable protocol and
artifact design—not empirical resolution rates, scanner rankings, or claims of
novel dynamic confirmation. The paper advances only if a predeclared,
disclosure-safe comparison shows that execution resolves a practically useful
fraction of disagreements without turning missing evidence into false certainty.

## 1. Research questions

- **RQ1:** How much do current open AI-agent, skill, and MCP security scanners
  agree after target, rule, affected surface, and claimed effect are normalized?
- **RQ2:** What fraction of disagreements can a separately observed authorized
  execution classify as confirmed, false under the declared test, not observed,
  or inconclusive?
- **RQ3:** How often does apparent multi-scanner agreement collapse after
  scanners are clustered by shared rule, signature, model, training data,
  template, or upstream implementation?
- **RQ4:** Which evidence deficits dominate inconclusive cases: unreachable
  path, missing environment, missing authorization, unstable model behavior,
  absent oracle, unsafe side effect, incompatible version, or incomplete trace?
- **RQ5:** What time, execution, model-call, human-review, and disclosure costs
  are added per correctly bounded finding?

## 2. Falsifiable hypotheses

### H1 - disagreement resolution

For eligible cases with a deterministic or independently reviewable effect
oracle, the execution protocol will resolve a predeclared non-trivial fraction
of cross-scanner disagreements while keeping unsupported terminal labels below
a predeclared error ceiling.

### H2 - independence correction

Treating scanners as mechanism-independent votes will overstate evidential
support relative to a dependency-aware analysis that groups shared mechanisms.

### H3 - value of abstention

A four-state adjudication protocol will have lower false-certainty risk than a
forced vulnerable/not-vulnerable label, at the cost of a measurable
inconclusive rate.

Thresholds are not chosen in this draft. They must be frozen after a pilot that
is excluded from the confirmatory evaluation. Failure is a publishable negative
result only if the protocol and analysis remain intact.

## 3. Claim and evidence model

A normalized scanner claim is:

```text
c = (target, target_version, surface, rule_family, claimed_precondition,
     claimed_effect, location, scanner, scanner_version, mechanism_cluster,
     source_evidence)
```

An execution observation is:

```text
o = (authorization, environment, procedure, attempt, observable_effect,
     oracle, evidence_refs, completeness, side_effects, limitations)
```

`o` is mechanistically independent only to the extent that its effect oracle
does not reuse the same signal that generated `c`. Running the scanner again,
asking the same model to judge its own finding, or counting duplicate rules is
not independent confirmation.

The decision function is intentionally partial:

```text
A(c, O) -> {
  confirmed-positive,
  false-positive-under-declared-test,
  not-observed-under-declared-test,
  inconclusive
}
```

- `confirmed-positive` requires an eligible, complete observation of the
  claimed effect under the declared conditions.
- `false-positive-under-declared-test` requires a valid test that directly
  contradicts a necessary scanner claim within the stated environment.
- `not-observed-under-declared-test` means the effect was not observed but the
  test does not justify contradiction or universal safety.
- `inconclusive` preserves missing, unsafe, unstable, or conflicting evidence.

This is not a truth oracle for all deployments and cannot prove a component
safe.

## 4. Why the contribution is not generic dynamic confirmation

Dynamic validation of static findings is established in research and patent
art. VIPER-MCP performs end-to-end confirmation for MCP taint vulnerabilities;
US8935794B2 describes dynamic unit-test validation of static findings; and
Trust but Verify compares vulnerability services with independent experiments.
The contribution tested here is empirical and infrastructural:

1. a multi-tool AI-agent disagreement corpus;
2. explicit claim identity and mechanism-dependency normalization;
3. bounded negative and inconclusive states;
4. portable, independently inspectable execution evidence;
5. measurement of which disagreement classes execution can and cannot resolve.

If the result is only “dynamic execution reduces false positives,” the paper is
not sufficiently new and must be stopped.

## 5. Corpus design

### 5.1 Strata

- agent skills and instruction packages;
- MCP server code/configuration and tool descriptions;
- agent configuration and permission files;
- memory/retrieval and indirect-prompt fixtures;
- tool-chain and data-flow fixtures;
- benign controls that intentionally resemble suspicious patterns;
- known vulnerable controls with safe deterministic effects;
- version-drift and missing-dependency cases;
- scanner-error and malformed-result cases.

### 5.2 Provenance and safety

- Synthetic fixtures must carry an answer-key digest and construction record.
- Public third-party fixtures require license, source revision, and dataset
  provenance.
- Real open-source components require a non-destructive test plan and
  coordinated disclosure before any affected identity or exploit detail is
  released.
- No public Internet target, real credential, uncontrolled account creation,
  irreversible transaction, or harmful payload is allowed.
- Scanner versions, rule packs, models, and configuration must be pinned.

### 5.3 Candidate scanners

Candidate open baselines include HackMyAgent/OASB, Snyk Agent Scan, Cisco Skill
Scanner, compatible static analyzers, and scanner-native benchmarks. Inclusion
depends on executable version pinning, license compatibility, non-network test
operation, and target-format overlap. Commercial scanners are not required and
must not be used without authorization and cost approval.

## 6. Required baselines

1. every individual scanner;
2. union of findings;
3. intersection of findings;
4. unweighted majority vote;
5. mechanism-cluster-weighted vote;
6. scanner-native confidence/severity ordering;
7. LLM-only adjudication over normalized finding text;
8. manual expert adjudication on a blinded sample;
9. static-to-dynamic confirmation baseline where a compatible implementation
   exists, including VIPER-MCP for its supported MCP taint subset;
10. full SOVA bounded execution protocol.

The study must report when a baseline is inapplicable instead of silently
dropping hard cases.

## 7. Endpoints

### Scanner behavior

- pairwise and multi-way Jaccard overlap after normalization;
- unique findings and rule-family coverage;
- precision, recall, macro-F1, false-positive rate, and false-negative rate on
  ground-truth strata;
- stability across repeated runs and version changes;
- mechanism-cluster diversity.

### Adjudication

- confirmed, contradicted-under-test, not-observed, and inconclusive rates;
- unsupported terminal-label rate;
- decision accuracy on planted ground truth;
- selective accuracy and coverage;
- calibration and risk-coverage curves where probabilistic outputs exist;
- execution/model calls, wall time, CPU/memory, and human-review minutes;
- side effects prevented, attempted, and approved;
- reproduction success on a second clean machine.

All metrics require confidence intervals. Scanner rankings must not collapse
different target coverage into one number.

## 8. Ablations

- remove mechanism-cluster information;
- convert four states to forced binary output;
- let scanner count act as a vote;
- reuse scanner evidence as the confirmation oracle;
- omit environment/version pinning;
- omit authorization and evidence completeness gates;
- remove `.sova-trace` verification;
- replace deterministic oracles with an LLM judge;
- omit repeated trials for stochastic model behavior.

## 9. Predeclared analysis

1. Freeze corpus, scanner revisions, normalization rules, claim families,
   budgets, and primary endpoints before the confirmatory run.
2. Run a disjoint pilot only to estimate variance, feasibility, and power.
3. Keep every eligible scanner output, including crashes and empty results.
4. Use paired analysis where the same target is evaluated by multiple methods.
5. Adjust exploratory multiple comparisons and label them separately.
6. Preserve negative and inconclusive cases in the released aggregate data.
7. Independently review a blinded sample of claim normalization and decisions.

## 10. Threats to validity

- Rule taxonomies may not align across tools.
- Scanner mechanisms may share undisclosed training data or signatures.
- Execution can fail to reach a vulnerable state and falsely appear negative.
- Synthetic fixtures can favor the SOVA oracle model.
- Publicly known vulnerabilities may be memorized by model-based scanners.
- Model/provider/version drift can alter scanner and target behavior.
- Safe effects may not represent production exploitability or harm.
- The same team implementing the protocol and fixtures can introduce bias.
- Disclosure constraints can prevent publication of the most informative cases.

## 11. Kill criteria

Stop the paper if:

- no representative multi-scanner dataset can be licensed and reproduced;
- scanner coverage overlaps too little for meaningful disagreement analysis;
- execution evidence is not mechanistically separable from scanner evidence;
- the result depends on one tool, one rule, or one synthetic family;
- unsupported terminal decisions exceed the frozen ceiling;
- a stronger existing study already contains the same dataset, protocol, and
  evidence semantics;
- the contribution reduces to SARIF import plus a dynamic test runner.

## 12. Current evidence

Completed engineering evidence:

- SARIF 2.1 import/export with scanner identity, rule, location, and evidence;
- four bounded adjudication states;
- duplicate terminal-observation rejection;
- mechanism-independence metadata;
- inert planning separated from reviewed execution observations;
- self-assessment watermark and no-send disclosure preparation;
- deterministic unit, integration, malformed-input, and safety tests.

Missing publication evidence:

- representative scanner corpus;
- pinned multi-scanner runs;
- safe execution observations;
- resolution rates and confidence intervals;
- faithful dynamic-confirmation baselines;
- blinded labels and independent reproduction.

## 13. Readiness

Fixed 100-point rubric used by the SOVA paper programme:

| Category | Score | Reason |
|---|---:|---|
| Precise research question and claim boundary | 9/10 | Questions and non-claims are explicit |
| Verified primary-source prior art | 9/10 | Strong paper, standard, tool, and patent sources screened; systematic review remains |
| Formal method and falsifiable mechanism | 12/15 | Decision and independence contracts specified; statistical thresholds await pilot |
| Executable implementation | 9/10 | Public contract and tests exist; multi-scanner runner is absent |
| Predeclared internal evidence | 3/15 | Engineering fixtures only; no disagreement corpus |
| Strong baselines and ablations | 2/10 | Designed but not executed |
| Independent or externally valid evidence | 0/15 | None |
| Manuscript and reproducibility package | 5/10 | Working manuscript/protocol only |
| Ethics, limitations, and IP gate | 5/5 | Explicit |
| **Total** | **54/100** | **HOLD submission** |

## 14. Publication and IP disposition

- Continue private dataset/protocol work: **GO**.
- Treat this document as a research working paper: **GO**.
- Submit, post to arXiv/ResearchGate, publish an article, or claim scanner
  superiority: **HOLD**.
- Broad patent on scanner normalization, dynamic confirmation, false-positive
  reduction, or bounded labels: **NO-GO**.
- New patent disclosure for Topic 16: **NO-GO on current mechanism**.
- Re-screen before any preprint because the field is moving rapidly.

No AI system is an author or inventor. No external action is authorized by this
draft.
