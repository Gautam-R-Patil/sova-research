# Evidence-Guided Trigger-Interaction Search for Stateful AI Agents

**Early working draft — not peer reviewed, not independently validated.** Published openly for transparency about research in progress; methods, results, and claims may change.

Version: 0.8, 2026-08-03

Proposed human author: Gautam R. Patil  
Affiliation: TRUSCOR Private Limited  
Contact: gautam@truscor.org

## Abstract

Conditional AI-agent failures can depend on interactions among identity,
conversation history, memory, retrieved content, tool results, runtime state,
and environment. Searching the Cartesian product is expensive, while prompt
mutation alone does not identify the causal interaction or produce a portable
reproduction artifact. We define a typed trigger-space model and introduce an
experimental evidence-factor-lattice search. The method measures bounded
near-miss evidence under single-factor interventions, ranks factor-value
effects, joins the strongest interventions into progressively higher-order
candidates, and separately minimizes a discovered trigger before packaging it
as a `.sova` behavior capsule with a canonical `.sova-trace`. On a predeclared
same-team synthetic corpus of 60 monotone triggers over six four-valued factors,
balanced across interaction orders two, three, and four, the method discovered
60/60 triggers within 32 discovery evaluations and exactly minimized 60/60.
The strongest deterministic baseline, pairwise covering, discovered 31/60;
eight-seed adaptive evolutionary and seeded-random baselines discovered
242/480 and 204/480 trials respectively. This result establishes an executable
mechanism on a mechanism-aligned grey-box corpus. It does not establish
black-box superiority, real-agent validity, novelty, non-obviousness, or
resilience to noisy, deceptive, non-monotone, sequential, or masked signals.
The paper therefore presents a falsifiable architecture, benchmark protocol,
and strict external-validation gate rather than a field-performance claim.

## Extended evidence synopsis

The implementation and study were frozen in
`private/experiments/topics-12-14-predeclared-protocol-2026-08-03.md` before
the retained result was generated. No network, model, GPU, harmful payload, or
live third-party system was used. The private prototype implements:

- a typed finite factor space;
- a four-state observation (`hit`, `near_miss`, `miss`, `inconclusive`);
- bounded scores that cannot themselves declare a harmful outcome;
- single-factor intervention probes from a declared baseline;
- factor-value ranking by score delta and evidence coverage;
- cumulative lattice joins over ranked factors;
- a distinct bounded minimization phase;
- immutable per-evaluation records suitable for trace conversion.

The result is deliberately narrow. The corpus exposes the exact fraction of
required factor assignments matched as a near-miss score. That information is
unlikely to be available from many real systems and is unusually favorable to
the proposed search. The study is useful because it tests the implementation,
budget, metrics, comparison harness, and failure boundaries. It is not a
substitute for predeclared real-agent experiments, independent labels, or
adversarial review.

## 1. Research question

Can observable, typed evidence about partial trigger satisfaction reduce the
number of executions needed to discover and minimize higher-order conditional
AI-agent behavior, compared with signature, seeded-random, pairwise-covering,
coverage-guided, and adaptive-evolutionary search under equal budgets?

The tested hypothesis is:

> When an authorized test environment supplies a calibrated, non-deceptive
> partial-evidence signal that varies with satisfied trigger factors, an
> intervention-and-join search will improve discovery of order-three and
> order-four interactions over baselines that do not use factor attribution.

The hypothesis is conditional. The method is not expected to dominate when:

- only a binary outcome is observable;
- the signal is noisy, adversarial, discontinuous, or unrelated to factors;
- the trigger requires strict action ordering rather than a state assignment;
- satisfying one factor masks another;
- the test cannot safely intervene on a factor;
- the target distribution differs materially from the synthetic corpus.

## 2. Problem and scope

Let a trigger space contain typed dimensions

$$
X = D_1 \times D_2 \times \cdots \times D_d,
$$

where each `D_i` may represent an actor identity, prompt feature, prior event,
memory state, retrieval result, tool observation, permission, runtime version,
time window, or environmental fixture. A test candidate is `x in X`. A target
returns an observable event/evidence record rather than hidden model thoughts.

The practical challenge is not merely finding any unusual text. A useful
system must:

1. preserve authorization and containment;
2. distinguish material behavior from a noisy proxy;
3. retain the exact factor assignments and causal execution lineage;
4. represent missing or invalid evidence as inconclusive;
5. minimize the discovered interaction without erasing required history;
6. package the result so another authorized environment can inspect,
   reproduce, compare, or falsify it.

This work does not claim to infer private chain-of-thought, model weights, or a
unique internal cause. It searches observable intervention space and records
only authorized prompts, outputs, tool actions, approvals, effects, artifacts,
and state transitions.

## 3. Relationship to prior work

Sleeper Agents demonstrates conditional model behavior activated by deployment
context and persistence through safety training. Watch Out for Your Agents and
Stateful Agent Backdoor extend the concern to planning stages, tool
observations, and cross-session state. These works motivate conditional search;
they do not make the existence of sleeper or stateful triggers a SOVA novelty.

GPTFuzzer and FuzzLLM automate jailbreak search through seed mutation,
templates, constraints, and judges. PAIR iteratively refines black-box prompts,
while TAP uses a pruned attack tree. These are strong prompt-space baselines.
Generic evolutionary mutation, attacker models, tree search, and evaluator
feedback are therefore excluded from the novelty boundary.

AgentDojo, InjecAgent, Agent Security Bench, and ToolEmu provide dynamic
tool-using environments, injection cases, broad agent-security taxonomies, and
emulated adverse environments. They establish that tools, memory, and
environmental observations belong in the search space and provide candidate
external corpora.

NIST SP 800-142 motivates t-way combinatorial testing because faults often
depend on small parameter interactions. Error-locating arrays and adaptive
faulty-interaction algorithms explicitly localize interacting factors.
Accordingly, typed factor spaces, pairwise coverage, adaptive interaction
localization, and sequence-covering arrays are not claimed as new.

AFLNet applies state and coverage feedback to stateful protocol fuzzing, FOX
uses fine-grained branch-distance feedback to direct mutations, and Delta
Debugging minimizes failure-inducing inputs. Coverage guidance, near-miss
distance, stateful mutation, and reduction are established techniques. The
research question is whether SOVA's evidence semantics and portable behavior
artifacts support a useful ordered combination for AI-agent experiments.

## 4. Evidence-factor-lattice method

### 4.1 Typed factors and baseline

Each dimension has a declared baseline value `b_i` and a finite authorized
value set. The full baseline is

$$
b=(b_1,\ldots,b_d).
$$

Changing a factor is an intervention in the test configuration, not a claim of
causation inside the model. Every candidate carries the changed-factor set,
procedure identity, environment fingerprint, and observation references.

### 4.2 Multi-valued observable

An execution produces

$$
O(x)=(s(x),c(x),q(x),E(x)),
$$

where `s` is one of hit, near-miss, miss, or inconclusive; `c` is a bounded
score in `[0,1]`; `q` is evidence coverage in `[0,1]`; and `E` references the
canonical evidence events. A hit requires the declared outcome oracle. A
near-miss score only schedules search; it can never prove that the target
behavior occurred. Missing required evidence produces inconclusive rather than
a zero score.

### 4.3 Single-factor intervention probes

For every non-baseline value `v in D_i`, evaluate `b[i <- v]` and compute

$$
\Delta_i(v) = c(b[i \leftarrow v]) - c(b) + \lambda
              (q(b[i \leftarrow v])-q(b)).
$$

The prototype uses a frozen `lambda` and deterministic tie-breaking. It keeps
the strongest admissible value `v_i*` for each dimension but preserves every
probe result. Negative, zero, missing, and contradictory signals remain
visible.

### 4.4 Ranked lattice join

Dimensions are ordered by decreasing `Delta_i(v_i*)`. Starting from the
baseline, the method cumulatively joins the strongest intervention for each
dimension:

$$
x_k=b[i_1\leftarrow v_{i_1}^*,\ldots,i_k\leftarrow v_{i_k}^*].
$$

Each `x_k` is executed until a hit, budget exhaustion, unsafe condition, or
inconclusive stop. For `d` factors with `m_i` values, the simple embodiment
uses at most

$$
1 + \sum_i (m_i-1) + d
$$

discovery executions before framework overhead. In the retained six-by-four
study it used 22 executions per successful case, below the 32-evaluation
budget.

This is a lattice traversal only in the operational sense of progressively
joining factor assignments. It is not claimed as a new mathematical lattice
theory.

### 4.5 Separate minimization

Once a hit is observed, minimization starts from the discovered candidate and
attempts to restore changed dimensions to baseline. A change is removed only
when a fresh execution retains the hit with sufficient evidence. The algorithm
records unsuccessful removals and never treats a near miss as retained failure.

Separating discovery from minimization prevents a search score from silently
becoming a reproduction claim. The minimized candidate is packaged only with
its fresh confirming trace, limitations, and budget history.

### 4.6 Output artifact

The output is not merely a trigger string. A `.sova` capsule can retain:

- the typed factor schema and selected values;
- portable scenario intent and executor alignment;
- authorization and safety boundaries;
- environment, model, tool, and runtime fingerprints;
- discovery and minimization histories;
- the minimized replay/reproduction recipe;
- observable assertions and evidence requirements;
- canonical `.sova-trace` references;
- uncertainty, unsupported dimensions, and limitations.

Trace playback, controlled re-execution, and semantic reproduction remain
separate operations. A successful search does not imply deterministic model
behavior or a universal causal explanation.

## 5. Experimental protocol

### 5.1 Corpus

The retained corpus contains 60 synthetic monotone conjunctions over six
dimensions with four values each:

- 20 order-two interactions;
- 20 order-three interactions;
- 20 order-four interactions;
- no harmful payloads;
- one planted satisfying assignment per required factor set;
- a score equal to the fraction of required assignments satisfied.

The last property makes the corpus grey-box and mechanism-aligned. It is both
the reason the experiment is informative about implementation and the central
reason it cannot support a general superiority claim.

### 5.2 Baselines

Six methods use the same 32 discovery and eight minimization evaluation
budgets:

1. known-signature candidates;
2. seeded random search over eight fixed seeds;
3. pairwise covering candidates;
4. coverage-guided deterministic scheduling;
5. adaptive evolutionary search over eight fixed seeds;
6. evidence-factor-lattice search.

The first five represent practical baseline families, not definitive
implementations of every cited system. The paper does not claim to reproduce
GPTFuzzer, TAP, AFLNet, FOX, or a specific covering-array library.

### 5.3 Measures

Primary measure: trigger discovery rate within the frozen budget.

Secondary measures:

- discovery rate by interaction order;
- mean and median attempts among successes;
- exact minimized-trigger rate among successes;
- false-positive and inconclusive rates;
- deterministic result stability under a repeated run.

The predeclared gate required the candidate to improve absolute discovery rate
by at least 0.10 over the strongest retained baseline, with zero false
positives on the synthetic corpus. Passing that internal gate does not satisfy
the external-publication gate.

### 5.4 Implementation

The CPU-only reference prototype is implemented in
`private/experiments/prototypes/factor_lattice_search.py`. The frozen runner is
`private/experiments/run_topic14_factor_lattice_study.py`, with focused unit
tests in `private/experiments/test_factor_lattice_search.py`. The retained JSON
result records environment, budgets, corpus definition, every aggregate, the
predeclared gate, and limitations.

## 6. Results

### 6.1 Discovery

| Method | Runs | Successes | Discovery rate | Mean attempts among successes |
|---|---:|---:|---:|---:|
| Evidence-factor-lattice | 60 | 60 | 1.000 | 22.00 |
| Pairwise covering | 60 | 31 | 0.517 | 10.94 |
| Adaptive evolutionary | 480 | 242 | 0.504 | 10.57 |
| Seeded random | 480 | 204 | 0.425 | 11.90 |
| Coverage guided | 60 | 19 | 0.317 | 13.00 |
| Known signature | 60 | 7 | 0.117 | 3.29 |

The candidate improved absolute discovery rate by 0.483 over the strongest
retained baseline and passed the frozen 0.10 internal gate. This comparison is
descriptive for the constructed corpus. Different run counts reflect the
predeclared eight seeds for stochastic methods; the per-run execution budget
is equal.

### 6.2 Interaction order

| Method | Order 2 | Order 3 | Order 4 |
|---|---:|---:|---:|
| Evidence-factor-lattice | 1.000 | 1.000 | 1.000 |
| Pairwise covering | 1.000 | 0.500 | 0.050 |
| Adaptive evolutionary | 0.894 | 0.494 | 0.125 |
| Seeded random | 0.844 | 0.350 | 0.081 |
| Coverage guided | 0.550 | 0.300 | 0.100 |
| Known signature | 0.200 | 0.150 | 0.000 |

The sharp order-three and order-four difference is consistent with the
candidate's access to factor-aligned near-miss information. It should not be
expected when such evidence is absent or misleading.

### 6.3 Minimization and error states

The candidate exactly minimized all 60 successful cases. Pairwise covering
exactly minimized 30/31 successes; adaptive evolutionary 218/242; seeded
random 180/204; coverage guided 18/19; and known signature 6/7. No method
reported a false positive or inconclusive result because the deterministic
synthetic oracle always returned complete evidence. These zeros do not measure
real-world false-positive or evidence-loss behavior.

### 6.4 Cost interpretation

The candidate was not fastest among successful cases. It spent 22 executions
deterministically to characterize single factors and construct the join,
whereas methods that happened to find a trigger sometimes succeeded sooner.
Its value in this study is discovery reliability for higher-order monotone
interactions, not minimum best-case latency.

## 7. Analysis and ablation hypotheses

The result supports three testable explanations:

1. factor attribution reduces the branching ambiguity faced by undirected
   mutation;
2. cumulative joins exploit monotonicity and therefore recover interactions
   beyond pairwise coverage;
3. a separate evidence-gated minimizer avoids accepting score-only reductions.

These explanations are not yet isolated experimentally. A confirmatory study
must ablate score delta, evidence coverage, factor ranking, cumulative joins,
and the separate minimizer. It must also include deceptive scores,
non-monotone triggers, ordered sequences, noisy repeated observations, missing
evidence, and cost-weighted factors.

## 8. Threats to validity

### Construct validity

The score exposes exactly how many planted requirements are satisfied. Real
agent systems may expose only final text, sparse tool effects, or unreliable
judges. The corpus therefore measures use of a favorable evidence signal, not
universal trigger discovery.

### Internal validity

The candidate, fixtures, labels, and baselines were authored by the same team.
The predeclared protocol reduces post-result flexibility but does not create
independence. Implementation defects could advantage one representation.

### External validity

No live model, browser, software platform, third-party agent, noisy API, or
cross-session system appears in this result. All triggers are monotone finite
conjunctions. Generalization is unknown.

### Statistical validity

The corpus has 60 deterministic cases rather than a sampled population, and
stochastic baselines use only eight seeds. The table supplies empirical rates,
not population-level confidence about real systems.

### Security and ethics validity

The study uses inert synthetic outcomes. Real trigger hunting can be dual use,
consume third-party resources, affect accounts, or cause side effects. SOVA
must require authorization, containment, side-effect ceilings, retention
controls, and human review. CAPTCHA bypass, unauthorized account creation, and
credential acquisition are outside the allowed design.

### Novelty validity

Combinatorial interaction testing, adaptive localization, coverage guidance,
branch distance, evolutionary fuzzing, and delta debugging are mature. The
paper's candidate contribution is the evidence semantics and integrated
portable workflow, not any one of those techniques. A patent conclusion
requires professional searching and claim analysis.

## 9. Reproducibility and artifact plan

The private research packet contains the protocol, source ledger, prototype,
unit tests, immutable JSON result, paper source, generated figures, PDF,
submission metadata, source archive, and build provenance. Before any public
artifact release, Gautam R. Patil must review the paper, select an IP strategy,
remove novelty-sensitive code if necessary, verify third-party licenses, and
approve the exact license and claims.

An external reviewer packet should expose only safe fixtures and should freeze:

- container or environment manifest;
- factor schemas and target cases;
- method implementations and seeds;
- per-method budgets;
- event and artifact digests;
- blind ground-truth release procedure;
- metrics and statistical plan;
- incident and disclosure procedure.

## 10. Required work before submission

1. Run a predeclared study on at least three real or independently authored
   agent systems spanning tool, memory, and multi-turn state triggers.
2. Use blind labels or a holdout corpus that the search implementation author
   cannot inspect.
3. Compare against faithful implementations of the strongest applicable
   prompt-fuzzing, combinatorial, directed-fuzzing, and reduction baselines.
4. Add noisy, deceptive, masked, non-monotone, ordered, and incomplete-evidence
   strata.
5. Report wall time, model calls, tokens, monetary cost, side effects, and
   failures, not only evaluations.
6. Perform ablations for score, coverage, ranking, joins, and minimization.
7. Obtain an independent security and dual-use review.
8. Complete founder and qualified patent-counsel review before preprint,
   source release, disclosure, or filing.
9. Verify every citation against the final manuscript and venue rules.
10. Record human contribution and inventorship evidence claim by claim.

## 11. Current gate decision

- Executable private prototype: **PASS-BOUNDED**.
- Frozen synthetic internal benchmark: **PASS**.
- Real-agent or independent external validation: **NOT DONE**.
- Private working-paper drafting: **GO**.
- Public preprint or article: **HOLD**.
- Broad evolutionary, combinatorial, near-miss, or minimization novelty claim:
  **NO-GO**.
- Narrow evidence-guided interaction-search patent screen: **HOLD FOR COUNSEL**.
- Patentability, novelty, non-obviousness, eligibility, and inventorship:
  **UNKNOWN**.

## References

1. Hubinger et al. Sleeper Agents: Training Deceptive LLMs that Persist Through
   Safety Training. arXiv:2401.05566, 2024.
2. Yang et al. Watch Out for Your Agents! Investigating Backdoor Threats to
   LLM-Based Agents. arXiv:2402.11208, 2024.
3. Stateful Agent Backdoor. arXiv:2605.06158, 2026.
4. Yu et al. GPTFuzzer: Red Teaming Large Language Models with Auto-Generated
   Jailbreak Prompts. arXiv:2309.10253, 2023.
5. Yao et al. FuzzLLM: A Novel and Universal Fuzzing Framework for Proactively
   Discovering Jailbreak Vulnerabilities in Large Language Models.
   arXiv:2309.05274, 2023.
6. Chao et al. Jailbreaking Black Box Large Language Models in Twenty Queries.
   arXiv:2310.08419, 2023.
7. Mehrotra et al. Tree of Attacks: Jailbreaking Black-Box LLMs Automatically.
   arXiv:2312.02119, 2023.
8. Debenedetti et al. AgentDojo: A Dynamic Environment to Evaluate Prompt
   Injection Attacks and Defenses for LLM Agents. NeurIPS 2024.
9. Zhan et al. InjecAgent: Benchmarking Indirect Prompt Injections in
   Tool-Integrated Large Language Model Agents. Findings of ACL 2024.
10. Zhang et al. Agent Security Bench. ICLR 2025.
11. Ruan et al. Identifying the Risks of LM Agents with an LM-Emulated Sandbox.
    ICLR 2024.
12. NIST SP 800-142. Practical Combinatorial Testing, 2010.
13. Colbourn and McClary. Locating and Detecting Arrays for Interaction Faults.
    Journal of Combinatorial Optimization, 2008.
14. Pham et al. AFLNet Five Years Later. arXiv:2412.20324, 2024.
15. Hsu et al. FOX: Coverage-Guided Fuzzing as Online Stochastic Control.
    arXiv:2406.04517, 2024.
16. Zeller and Hildebrandt. Simplifying and Isolating Failure-Inducing Input.
    IEEE Transactions on Software Engineering, 2002.

## Topic 17 prior-art update - 2026-08-03

The later Topic 17 screen identified two direct 2026 sources that further
narrow this paper. *Benign in Isolation, Harmful in Composition* (SCR-Bench,
arXiv:2606.15242) defines and measures agent-skill composition risk across
capability-flow, trust-transfer, and authorization-confusion paths.
*AgentThread* (arXiv:2606.28690) reports protocol-composition failures using
formal invariants and executable counterexamples. Accordingly, P1 must not
claim novelty for composition-only failures, activated-path testing, or
cross-protocol composition effects.

P1 remains limited to the evidence-guided trigger-interaction search question.
Composition failure attribution is now a separate combined Topic 15+17
candidate, P7. Any future P1 experiment using SCR-Bench or AgentThread artifacts
must cite them as strong direct baselines and preserve their native task
semantics.
