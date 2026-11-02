# Evidence-Bounded Causal Composition Forensics

## Minimal Responsibility Sets for Partially Observed AI-Agent Failures

**Early working draft — not peer reviewed, not independently validated.** Published openly for transparency about research in progress; methods, results, and claims may change.

- Version: 0.2, 2026-08-03
- Proposed human author: Gautam R. Patil
- Affiliation: TRUSCOR Private Limited
- Contact: gautam@truscor.org
- Status: `GO-PROSPECTUS / GO-PRIVATE-PROTOTYPE / HOLD-MANUSCRIPT / HOLD-IP / HOLD-SUBMISSION`

## Abstract

AI-agent failures can arise from an ordered interaction among models, prompts,
memory, retrieval, tools, permissions, identities, handoffs, and external
services. Existing work now covers step attribution, counterfactual replay,
intervention-driven debugging, full-trace benchmarks, skill composition risk,
protocol-composition failures, combinatorial interaction testing, and
failure-inducing-input minimization. A defensible SOVA contribution therefore
cannot be “replay the failure,” “find the bad step,” or “test benign components
together.”

We propose an evidence-bounded research architecture for a narrower problem:
given a partially ordered, partially observed execution and a composition-only
effect, return a set of minimal responsibility hypotheses whose supporting
interventions preserve declared context and meet claim-specific evidence
requirements. The method must preserve multiple sufficient causes,
confounding, impossible interventions, missing sensors, and inconclusive
results. It must not infer hidden chain-of-thought or turn a successful repair
into unique causal proof. Each supported hypothesis is bound to the original
and counterfactual traces and can be packaged as a portable `.sova` behavior
capsule for independent inspection and reproduction.

Separate public SOVA components implement partial-order reconstruction, paired
layer interventions, abstaining attribution, composition search, isolated
negative controls, and fresh element-removal tests. The integrated research
architecture, adaptive intervention scheduler, multi-element responsibility
sets, and real-system evaluation do not yet exist. This draft specifies the
falsifiable gap and the experiments required before any novelty or superiority
claim.

## 1. Research questions

- **RQ1:** Under incomplete and multi-clock observation, can claim-specific
  evidence closure reduce false causal support relative to passive, judge-only,
  and intervention methods that do not enforce evidence sufficiency?
- **RQ2:** Can minimal responsibility *sets* represent interaction failures and
  multiple sufficient repairs more faithfully than a forced single agent/step
  label?
- **RQ3:** Can an evidence- and budget-aware intervention scheduler recover the
  same supported responsibility sets with fewer re-executions than exhaustive
  subset interventions, random scheduling, pairwise/t-wise testing, and
  similarity-prioritized testing?
- **RQ4:** How do missing sensors, clock uncertainty, stochastic policies,
  non-equivalent environments, unsafe interventions, and multiple causes affect
  selective accuracy, coverage, and cost?
- **RQ5:** Can a portable capsule preserve enough context, evidence, and
  limitations for a second machine to recompute the decision without treating
  the capsule as proof that its sensors were truthful?

## 2. Non-claims

This work does not claim to:

- capture private model thoughts or hidden chain-of-thought;
- infer a unique internal model cause from black-box behavior;
- invent structural causal models, do-interventions, Shapley values,
  counterfactual replay, or causal graphs;
- invent composition-only failures, skill-composition risk, covering arrays,
  ordered combinatorial tests, adaptive fault localization, or delta debugging;
- prove sensor truth, organizational blame, legal causation, or universal
  safety;
- guarantee that the proposed combination is novel, non-obvious, eligible, or
  patentable.

## 3. Formal problem

### 3.1 Observed execution

Represent a run as a typed partial-order event graph:

```text
G = (V, E_causal, E_local, K, M)
```

where `V` are observable events, `E_causal` are declared causal links,
`E_local` are local monotonic-order links, `K` describes clock domains and
uncertainty, and `M` records missing, redacted, omitted, or dropped evidence.
No arbitrary cross-clock total order is invented.

### 3.2 Composition hypothesis

A composition hypothesis `H` is an ordered typed sub-hypergraph over components
and interactions:

```text
H = (N_H, A_H, order_H, state_H, effect_H)
```

It can include model, policy, memory, retrieval, tool, identity, permission,
handoff, MCP, and environment elements. Hyperedges allow one effect to require
several jointly present inputs rather than forcing a pairwise graph fiction.

### 3.3 Claim-specific evidence closure

For claim `q`, define a versioned requirement function `R_q`. The evidence
closure is:

```text
C_q(G, H) = the minimal observable event/effect set required by R_q
            to evaluate H under the selected threat model
```

If closure is incomplete, attribution cannot be supported. A model summary,
scanner assertion, or generated rationale does not fill a missing deterministic
effect event.

### 3.4 Eligible intervention

An intervention `I` changes a declared set of elements and carries an
equivalence envelope:

```text
I = (changed_elements, preserved_context, expected_effect,
     authorization, safety_bound, trace_links)
```

It is eligible only when:

1. the baseline effect reproduced;
2. changed elements are exactly declared;
3. required preserved context satisfies the versioned equivalence policy;
4. evidence closure is complete for baseline and intervention;
5. execution is authorized and inside the declared side-effect ceiling; and
6. original and counterfactual evidence are linked and independently
   verifiable under the selected integrity policy.

### 3.5 Multi-valued intervention result

```text
Q(I) in {
  prevents,
  persists,
  confounded,
  impossible,
  inconclusive
}
```

Repeated eligible trials produce an effect estimate and interval. No single
flip is promoted to causal proof.

### 3.6 Minimal responsibility hypothesis

A set `S` is a supported minimal responsibility hypothesis for declared effect
`y` only if a predeclared joint intervention on `S` prevents `y` with sufficient
evidence and every tested proper subset fails to meet that same support rule.
Multiple distinct minimal sets may coexist. An untested subset remains unknown,
not non-causal.

The output is therefore:

```text
R = (supported_sets, contradicted_sets, unresolved_sets,
     equivalence_limits, evidence_gaps, uncertainty, budget_exhaustion)
```

This is a bounded experimental conclusion, not metaphysical or legal causation.

## 4. Candidate private architecture

The working name is **Evidence-Closed Responsibility Hypergraph (ECRH)**. The
name is provisional and carries no novelty claim.

### Phase A - reconstruction

1. verify trace/package integrity under the selected policy;
2. construct the typed partial-order event graph;
3. preserve missing parents, uncertain clocks, dropped events, redactions, and
   imported-evidence trust state;
4. identify the declared effect and candidate composition subgraph.

### Phase B - hypothesis frontier

1. seed candidate layer, edge, node, order, state, and joint-interaction
   hypotheses from the composition graph and observed decision points;
2. reject candidates whose required intervention is unauthorized or unsafe;
3. compute evidence-closure deficits before execution;
4. preserve multiple candidate sets instead of forcing one top cause.

### Phase C - bounded intervention scheduling

The scheduler may use only admissible information:

- expected partition of the remaining hypothesis set;
- evidence-closure deficit likely to be resolved;
- execution, model-call, time, and human-review cost;
- side-effect and authorization risk;
- stochastic repeat requirement; and
- portable re-execution feasibility.

A candidate acquisition function for private testing is:

```text
score(I) = expected_hypothesis_reduction(I)
           * expected_evidence_completion(I)
           / (execution_cost(I) + risk_penalty(I) + epsilon)
```

The terms must be estimated without using hidden answer keys in confirmatory
evaluation. The formula is a hypothesis, not new mathematics.

### Phase D - fresh minimality tests

After a supported joint intervention, evaluate proper subsets with fresh
evidence. Cached observations may be reused only when the complete environment,
procedure, trace integrity, and equivalence envelope match. Missing or
non-equivalent subset evidence blocks minimality.

### Phase E - portable output

Package:

- the original scenario and effect contract;
- typed composition path/hyperpath;
- intervention definitions and equivalence envelopes;
- original and counterfactual trace references;
- evidence-closure policy and deficits;
- supported, contradicted, and unresolved sets;
- uncertainty, budgets, unsafe/impossible cases, and limitations;
- migration-safe provenance and content digests.

The `.sova` capsule organizes the study; `.sova-trace` remains the low-level
event/evidence stream.

## 5. Relationship to current SOVA implementation

Already implemented publicly:

- verified native-trace and explicit-trust external-event reconstruction;
- partial-order timelines, clock/missing-sensor markers, and evidence links;
- paired one-layer interventions with equivalence/evidence eligibility;
- prevention rates, Wilson intervals, confound/impossible/inconclusive states;
- typed composition graphs, pairwise/t-wise/risk/ordered planning;
- isolated negative controls, fresh element removals, and portable fragments;
- deterministic fixtures for clean, confounded, stochastic, missing-sensor,
  and composition-only cases.

Not implemented:

- hyperedges and joint responsibility sets;
- evidence-closure computation per causal claim;
- hypothesis-frontier maintenance;
- information/evidence/cost/risk intervention acquisition;
- exhaustive, CAR, DoVer, REFLECT, and faithful attribution baselines;
- joint-subset minimality with stochastic repeated trials;
- public benchmark adapters and independent reproduction.

## 6. Strongest baselines

### Passive and learned attribution

- passive frequency/topology heuristics;
- direct LLM attribution;
- RAFFLES;
- AgenTracer/StepFinder or the strongest reproducible learned localizer;
- ErrorProbe/VerifyMAS where artifacts and licenses permit.

### Interventional attribution

- single candidate patch-and-rerun;
- DoVer;
- REFLECT;
- Causal Agent Replay contrastive and Shapley estimators;
- exhaustive single-element and feasible subset intervention.

### Composition search and localization

- random connected paths;
- connected-pair and t-wise covering arrays;
- NIST-compatible sequence coverage where applicable;
- risk-weighted path ordering;
- failing-case similarity reprioritization;
- SCR-Bench path evaluation;
- AgentThread formal/executable counterexample workflow;
- delta debugging/minimal failure-inducing set reduction.

If faithful reproduction of a baseline is impossible, the paper must disclose
the gap and cannot claim superiority to it.

## 7. Experiment programme

### 7.1 Synthetic causal hypergraph corpus

Generate sealed cases varying:

- component count and path length;
- causal interaction order;
- one versus multiple sufficient responsibility sets;
- confounding and correlated non-causes;
- stochastic effect probability;
- sensor omission, redaction, and dropped-event patterns;
- independent clock domains and skew;
- non-equivalent environment drift;
- unsafe or impossible interventions;
- monotone, non-monotone, masked, and order-dependent effects.

Answer keys must remain sealed from scheduling methods. Generation code,
protocol, and dataset digest are frozen before retained runs.

### 7.2 Public attribution benchmarks

Evaluate compatible subsets or explicit translations of TraceElephant,
Who&When Pro, TracerTraj, and other licensed benchmarks. Preserve their native
tasks and report any information lost during conversion.

### 7.3 Composition benchmarks

Evaluate SCR-Bench, AgentThread artifacts, and compatible Agent Security Bench
cases. SOVA must not silently relabel isolated-component assumptions or claim
that all benchmark effects are vulnerabilities.

### 7.4 Real-agent study

Run multiple model families and at least two independently operated agent
frameworks in authorized local/sandboxed fixtures. No provider account is
created and no money is spent without the user's explicit authority. Models,
prompts, tools, environments, retries, and budgets are pinned.

### 7.5 Independent validation

- independent case authoring for a held-out stratum;
- blind attribution labels and disagreement resolution;
- separate reproduction on a clean machine;
- independent security/dual-use and privacy review;
- checksum-pinned artifact package.

## 8. Endpoints

- exact and set-valued responsibility precision/recall;
- selective accuracy, coverage, and area under the risk-coverage curve;
- false-support rate under missing evidence;
- supported-set minimality rate;
- recovery of multiple sufficient sets;
- interventions, model calls, wall time, CPU/memory, and human-review cost;
- unsafe/impossible intervention avoidance;
- calibration of reported support intervals;
- cross-machine recomputation and reproduction rate;
- performance by missingness, nondeterminism, interaction order, and path
  length.

Primary endpoints and minimum effects must be frozen after a disjoint pilot and
power analysis.

## 9. Required ablations

- remove evidence closure;
- force a total timeline across clocks;
- force a single cause;
- convert multi-valued results to binary labels;
- remove equivalence checking;
- allow cached/non-fresh minimality evidence;
- remove risk and safety terms from scheduling;
- remove hypothesis-reduction term;
- remove evidence-completion term;
- omit repeated trials;
- omit portable recomputation.

## 10. Threats to validity

- Synthetic graphs may encode the proposed representation too directly.
- Benchmark conversions can leak labels or change the causal question.
- Intervention policies can alter downstream stochastic distributions.
- Multiple repairs do not necessarily imply multiple actual causes.
- Missing sensors can be non-random and adversarial.
- A selected causal layer may be an abstraction over several mechanisms.
- Model/provider drift can invalidate repeated-run comparisons.
- Same-team fixtures and labels can overstate specification consistency.
- Portable evidence proves integrity only within its threat model, not sensor
  truth or non-repudiation.

## 11. Kill criteria

Stop the paper or reduce it to an engineering report if:

- the full method does not improve selective risk, evidence-gap handling, or
  intervention cost over the strongest compatible interventional baseline;
- benefits disappear on independently authored or public cases;
- responsibility-set outputs are not more faithful than single-step outputs;
- the scheduler relies on sealed ground truth or mechanism-aligned signals not
  available in real systems;
- the method cannot distinguish non-equivalence from causal effect;
- the paper reduces to CAR plus covering arrays plus delta debugging without a
  measured new technical effect;
- a later source contains the same complete architecture and evaluation.

## 12. Current evidence

The current five-case deterministic attribution fixture has two supported clean
single-layer cases and three correct abstentions for confounding,
stochastic/incomplete evidence, and a missing sensor. Raw top-one accuracy is
0.4; selective accuracy and predeclared decision accuracy are 1.0; supported
coverage is 0.4. A passive-frequency baseline scores 0.0 on the deliberately
discriminating fixture.

The current composition fixture finds and minimizes one ordered
memory-to-agent-to-tool effect while every component is negative in its
declared isolated test. Every retained node and edge is necessary under fresh
removal observations.

These results validate engineering paths only. They are too small,
same-team, deterministic, and mechanism-aligned to establish a research claim.

## 13. Readiness

| Category | Score | Reason |
|---|---:|---|
| Precise research question and claim boundary | 9/10 | Combined gap and non-claims are explicit |
| Verified primary-source prior art | 10/10 | Direct scholarly, standards, and patent art screened |
| Formal method and falsifiable mechanism | 10/15 | Architecture specified; estimator and policy remain prototypes |
| Executable implementation | 5/10 | Separate Topic 15/17 components exist; integrated ECRH does not |
| Predeclared internal evidence | 3/15 | Only small engineering fixtures |
| Strong baselines and ablations | 1/10 | Designed but not run |
| Independent or externally valid evidence | 0/15 | None |
| Manuscript and reproducibility package | 4/10 | Working prospectus only |
| Ethics, limitations, and IP gate | 5/5 | Explicit |
| **Total** | **47/100** | **HOLD submission** |

## 14. Paper and IP disposition

- Continue private formalization, prototype, and benchmark work: **GO**.
- Treat P7 as the single combined Topic 15+17 research candidate: **GO**.
- Separate broad “counterfactual agent forensics” paper: **NO-GO as a novelty
  claim**.
- Separate broad “benign in isolation, harmful in composition” paper:
  **NO-GO due direct prior art**.
- Public novelty/superiority claim or submission: **HOLD**.
- Narrow integrated patent candidate: **HOLD FOR HUMAN CONCEPTION RECORD,
  REDUCTION TO PRACTICE, COMPARATIVE TECHNICAL EFFECT, PROFESSIONAL SEARCH,
  DISCLOSURE ANALYSIS, AND COUNSEL**.
- Broad causal replay, composition testing, covering array, risk ordering, or
  minimization patent: **NO-GO**.

No AI system is an author or inventor. No external disclosure or filing is
authorized by this draft.
