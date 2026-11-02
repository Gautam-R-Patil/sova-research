# `.sova`: Evidence-Bounded Semantic Reproduction of Observable AI Behavior Across Runtimes

**Early working draft — not peer reviewed, not independently validated.** Published openly for transparency about research in progress; methods, results, and claims may change.

Version: 0.96, 2026-08-03  
Author: Gautam R. Patil  
Affiliation: TRUSCOR Private Limited  
Contact: gautam@truscor.org

## Abstract

AI-system failures are commonly shared as prompts, screenshots, logs, or
provider-specific traces. Such artifacts rarely state which observable outcome
must recur, which environmental differences are material, what evidence is
required, or how unsafe, incompatible, and inconclusive trials affect a
reproduction estimate. We present an experimental `.sova` AI-behavior capsule
and study a narrower evidence-bounded semantic-reproduction mechanism. The
mechanism combines a typed observable-outcome contract, execution-context
equivalence envelope, evidence-sufficiency gate, multi-valued decision, and
recomputable repeated-trial record while separating trace playback, controlled
re-execution, and semantic reproduction.

On a preregistered same-team corpus of 144 pairs across six capsule profiles,
the implementation matched all constructed labels; four simple structural
baselines had macro-F1 from 0 to 0.111. The first Wilson-interval rule failed a
predeclared analytic coverage gate (minimum 0.914), and the retained amendment
used an exact Clopper-Pearson interval (minimum 0.956). A 12-case, 24-run
no-Atlas study completed two-backend execution, packaging, playback,
comparison, and offline verification. Thirteen official Inspect AI JSON logs
first exposed a transcript-only adapter defect (5/13 accepted); the corrected
adapter converted all thirteen as explicitly lossy and emitted 280/280 verified
events. A retrospective external cohort of 22 real-model WebArena trajectories
yielded eleven paired decisions under AgentRewardBench labels. A post-result
study then added a deterministic WebGraphEval-style action-graph baseline and
a calibrated Model2Vec semantic-text baseline. The graph baseline matched 5/7
conclusive labels and missed both equivalent pairs; the semantic baseline
matched 7/7 conclusive labels but, like the graph baseline, could not represent
the four inconclusive cases. Its probability calibration was fitted on STS
Benchmark training labels and evaluated without refitting on 1,379 held-out
human-labelled sentence pairs (Brier 0.1267; 10-bin ECE 0.0382).

A later frozen post-build study exercised the public Topic 12 contract over
12 observable contracts and 96 candidate traces. Every contract received the
predeclared vector of four reproduced, two not-reproduced, and two
inconclusive decisions; zero incomplete traces were accepted as reproduced
and zero candidate traces were byte-identical to their references. Each
eight-trial aggregate remained inconclusive because mandatory evidence was
missing from two trials. This demonstrates conservative public composition,
not independent validation of the richer private mechanism.

These results establish internal implementability and useful negative results,
not independent cross-runtime validity. Same-team labels, one physical host,
retrospective trajectories, absent controlled real-model re-execution, and no
independent blind cohort prevent a general superiority or novelty claim. The
paper therefore supplies a frozen 120-case blind-review protocol and remains on
submission HOLD until its external validation gates pass.

## Extended evidence synopsis

AI-system failures are commonly shared as prompts, screenshots, logs, or
provider-specific traces. These artifacts often permit inspection but do not
state which observable outcome must recur, which environmental changes are
acceptable, which evidence is necessary to support the comparison, or how
incompatible and inconclusive trials affect a reproduction rate. We introduce
an experimental architecture for `.sova`, a portable AI-behavior capsule, and
study a narrower mechanism for evidence-bounded semantic reproduction.

The mechanism combines a typed observable outcome contract, an execution-
context equivalence envelope, evidence-availability constraints, a multi-valued
reproduction decision, and recomputable repeated-trial estimates. It
distinguishes trace playback, controlled re-execution, and semantic
reproduction and never relies on hidden chain-of-thought. In a preregistered
synthetic study of 144 labeled behavior pairs spanning six capsule profiles,
the reference mechanism exactly recovered the constructed ground truth
(macro-F1 1.0), while four deliberately simple baselines obtained macro-F1
between 0 and 0.111. A first Wilson-interval gate failed in the worst
predeclared cell (analytic coverage 0.914); a disclosed post-result amendment
added an exact Clopper–Pearson interval, whose minimum analytic grid coverage
was 0.956. A separate 12-case, 24-trace no-Atlas study completed capsule
inspection, two-backend execution, inert playback, declared-outcome comparison,
capsule repackaging, and independent offline verification.
In a post-result seven-case integration study using verified public capsules
and traces, the mechanism matched all constructed decisions, compared with
5/7 for declared-observable equality, 3/7 for normalized request/outcome
interaction equality, 2/7 for terminal completion, and 1/7 for byte equality.
Pinned AgentEvals 0.0.9 strict, unordered, superset, and
graph-strict trajectory matchers each matched 2/7. A bounded converter pinned to Inspect AI
source commit
`1ea01a9e1b3c94db3cdf93ce5583ffebf86e159f` matched four constructed
JSON-log decisions while explicitly reporting conversion loss. These
additional studies were not preregistered and use same-team synthetic cases.
The adapter was then tested against thirteen pinned official Inspect JSON
logs. An initial 5/13 result falsified a transcript-only assumption; explicit
support for three official source layouts produced 13/13 lossy imports and
280/280 verified emitted events on the unchanged corpus. Separately,
twenty-two real-model WebArena trajectories from two model families and eleven
shared tasks were paired using external AgentRewardBench expert labels. They
produced two equivalent, five divergent, and four inconclusive comparisons;
seven source trajectories contained expert disagreement. Three Harness Evals
0.15.0 baselines matched 3/7, 2/7, and 2/7 frozen constructed labels versus
7/7 for the candidate. Stable private decisions also repeated with identical
canonical digests on CPython 3.11 through 3.14 on one host.
A further post-result study adapted WebGraphEval's graph-construction idea to a
deterministic two-pass edit-similarity graph and evaluated a CPU-only Model2Vec
semantic baseline. On the eleven AgentRewardBench pairs, both baselines were
forced to map all four inconclusive cases to divergent. Among the seven
conclusive pairs, graph comparison matched 5/7 and semantic comparison matched
7/7. The semantic score's Platt calibration was trained on 5,749 STS Benchmark
pairs, checked on 1,500 validation pairs, and tested without refitting on 1,379
pairs (accuracy 0.8216, Brier 0.1267, ECE10 0.0382, Pearson 0.7726, Spearman
0.7541). This out-of-domain calibration does not validate semantic reproduction
labels.

These results demonstrate internal implementability and falsify byte equality
and ordinary structural equality as sufficient reproduction criteria on the
tested cases. The external cohort establishes a bounded real-model
retrospective comparison, not controlled re-execution, purpose-built blind
SOVA labels, environment equivalence, independent adoption, novelty, or
general superiority. Publication is therefore held pending independent
controlled-runtime and blind-review evidence.

## 1. Research question

Can a portable AI-behavior artifact make cross-runtime reproduction decisions
that remain:

1. explicit about the observable behavior being tested;
2. sensitive to safety, incompatibility, missing evidence, and invalid
   provenance;
3. tolerant only to declared non-material variation;
4. recomputable from retained per-trial evidence; and
5. more informative than exact trace, final-output, event-kind, or terminal-
   status equality?

The falsifiable hypothesis is that the complete contract will reduce false
semantic equivalence and false semantic divergence against these baselines on
held-out, independently labeled cases. The current synthetic corpus tests
implementation consistency, not the final hypothesis.

## 2. Artifact model

`.sova` is a lightweight, versioned, inert-by-default AI-behavior capsule. A
capsule can package or reference scenarios, triggers, environments,
dependencies, models, agents, tools, traces, fixtures, assertions,
evaluations, findings, hypotheses, provenance, integrity, citation,
disclosure, and authorship records.

`.sova-trace` is the canonical low-level observable event and evidence stream.
The capsule organizes material required to inspect, replay, reproduce, compare,
study, verify, or cite a behavior. It is not a claim that a model's private
thoughts were captured.

Three operations are disjoint:

- **trace playback** renders an inert recorded timeline;
- **controlled re-execution** performs a fresh authorized run and creates a new
  trace;
- **semantic reproduction** evaluates whether a declared material outcome
  recurs under an explicit context and evidence contract.

## 3. Proposed mechanism

### 3.1 Typed observable outcome contract

A contract contains required and optional predicates over observable event
families. The current deterministic prototype supports existence, absence,
equality, containment, minimum count, and ordering. Each predicate identifies
the event family, value path, expected relation, and evidence events used.

A missing required event family produces `unknown`, not `fail` or `pass`.
Optional future judges must record identity, version, inputs, configuration,
calibration, uncertainty, and disagreement. The integrated contract now
supports typed human/model/rule adjudication records with pseudonymous
adjudicator identity, label, blinding and implementation-independence
assertions, integer confidence basis points, and private-rationale digests. It
reports unanimous, plurality, tie, or absent agreement and never silently
overrides the evidence-derived outcome. This implements the record structure;
it does not create independent reviewers or validate their assertions.

### 3.2 Execution-context equivalence envelope

Each context dimension declares one comparison mode:

- `exact`: values must match;
- `one-of`: the candidate must belong to an allowed set;
- `tolerance`: a numeric deviation is bounded;
- `record`: divergence is recorded but does not block comparison;
- `ignore`: the dimension is explicitly non-material.

Missing required context produces an unknown comparison. This prevents a
receiver from silently treating an absent model, tool, protocol, or dependency
version as equivalent.

### 3.3 Evidence-bound decision

The decision state is one of:

- `equivalent`;
- `divergent`;
- `inconclusive`;
- `incompatible`;
- `unsafe`;
- `evidence-invalid`.

Safety and evidence validity are checked before behavior predicates.
Incompatible context is not counted as ordinary behavioral failure. An
otherwise matching behavior is inconclusive when required context equivalence
is unknown.

### 3.4 Reproduction-rate record

Every trial retains:

- trial identity;
- outcome state;
- environment stratum;
- evaluator identity;
- evidence digest.

The summary reports counts for every state, the success rate among conclusive
trials, Wilson and exact binomial intervals, and a conservative partial-
identification range in which inconclusive trials may be either success or
failure. Incompatible, unsafe, and invalid-evidence trials remain visible but
are not silently folded into a Bernoulli denominator.

Correlated runs must be grouped and modeled rather than counted as independent
replicates. A production study should use a hierarchical or generalized linear
mixed model when model, item, environment, and judge variation warrant it.

## 4. Relationship to prior work

The design does not claim novelty for portable archives, traces, workflow
replay, deterministic replay, provenance, semantic evaluation, behavioral
contracts, repeated trials, or statistical intervals.

ReproEvalCard specifies reproducibility-critical material for LLM evaluation
pipelines. `agrepl` records and deterministically replays external interactions.
WebGraphEval compares agent trajectories through canonical graphs. ReliableEval
addresses stochastic and prompt-sensitive evaluation. Semantic-Eval studies
semantic generation metrics. ContractEval and Phoenix demonstrate explicit
behavioral-contract evaluation. PoC-Adapt uses structured semantic oracles for
vulnerability reproduction. W3C PROV defines normalization, validity, and
equivalence for provenance instances. NIST AI 800-2 and 800-3 require explicit
uncertainty assumptions and motivate hierarchical statistical models.

The candidate contribution is therefore the narrower coupling of observable
outcome semantics, context equivalence, evidence sufficiency, multi-valued
reproduction states, and recomputable uncertainty inside a portable cross-
runtime behavior artifact.

## 5. Experimental protocol

The protocol was frozen privately before corrected-mechanism results.

### 5.1 Synthetic behavior corpus

- six profiles: security, evaluation, agent trajectory, behavioral
  interpretability, incident forensics, and research publication;
- 24 cases per profile, 144 total;
- balanced ground-truth classes across equivalent, divergent, inconclusive,
  incompatible, unsafe, and evidence-invalid;
- typed label, causal-order, and forbidden-egress predicates;
- context drift in model family, runtime, protocol, temperature, and host.

### 5.2 Baselines

1. exact canonical trace equality;
2. exact final observable payload equality;
3. terminal completion-status equality;
4. normalized request/outcome interaction equality;
5. untyped event-kind equality.

These are intentionally simple lower baselines, not substitutes for direct
integration with WebGraphEval, semantic metrics, or full replay systems.

### 5.3 No-Atlas execution study

Twelve owned deterministic conditional fixtures alternated trigger and control
inputs. Each capsule ran through `ScriptedExecutor` and
`RestrictedLocalExecutor`, yielding 24 traces. Each case required:

- successful bounded execution;
- passing observable oracle;
- declared-outcome equivalence across executor mechanics;
- native and dependency-free offline trace verification;
- inert playback;
- an evidence capsule containing scenario plus both traces.

No network, external credential, live target, Atlas MCP, or model provider was
used.

### 5.4 Post-result integrated and Inspect baseline study

After the preregistered study, protocol amendment 0.5 connected the private
mechanism to actual public `.sova` packages, verified `.sova-trace` evidence,
both no-Atlas executors, deterministic migration, an inert renderer, and a
private CLI. Seven controlled cases covered cross-executor equivalence,
repeat equivalence, required-context incompatibility, an unsafe comparison, a
partial prefix, provider success contradicted by the SOVA oracle, and a
complete run lacking mandatory evidence.

The same traces were converted to AgentEvals 0.0.9's documented OpenAI-style
message/tool-call representation and evaluated by its deterministic strict,
unordered, and superset trajectory matchers with exact tool arguments. The
package is MIT licensed and the evaluation used no model, key, or network
call. This is a mature-library structural-trajectory baseline, but the seven
labels and conversion remain same-team.

A separate adapter was pinned to Inspect AI repository commit
`1ea01a9e1b3c94db3cdf93ce5583ffebf86e159f` and EvalLog version 2. It converted
a bounded synthetic JSON-dump subset into SOVA events with an explicit
fidelity report. Four cases varied surface output, source score, and score
availability. The adapter does not parse Inspect's binary `.eval` format and
the initial direct-baseline experiment did not use a third-party real log.

### 5.5 Pinned external corpora and implementation baselines

The Inspect adapter was run against every JSON EvalLog in four frozen official
test-log directories at repository commit
`1ea01a9e1b3c94db3cdf93ce5583ffebf86e159f`. The thirteen-file corpus contains
transcript events, legacy flat events, no-event-stream records, and logs with
no samples. Adapter 0.2 accepted five files and typed-refused eight because it
assumed `sample.transcript`. That negative result was retained. Adapter 0.3
added explicit layout detection and was rerun against the unchanged corpus.
Raw official fixture bytes are not redistributed.

The AgentRewardBench authors' official demo Space at commit
`271f96552e2e6dd13fe8e3572f0e5264039f6fd0` contained twenty-two cleaned
WebArena trajectories: two real-model agent families over eleven shared task
identifiers. Every trajectory was converted into a verified trace/capsule
using only task/model provenance, action verb/digest, and grouped expert
success, side-effect, optimality, and looping labels. Goal text, raw reasoning,
action arguments, screenshots, accessibility trees, URLs, chat messages, and
annotator identity were excluded and probed in decoded retained structures.
The two models were paired by task under the predeclared multi-valued rule.

Harness Evals 0.15.0 supplied three separately maintained deterministic
baselines over the unchanged seven-case constructed corpus: `ExactMatch`,
`ToolArgumentMatch`, and `ToolCorrectness`. The study used no model, account,
credential, provider, or network call.

The Space metadata did not assert a source-data licence, so source bytes are
not redistributed. The trajectory study is retrospective and the SOVA
interpretation remains same-team. It does not establish controlled
re-execution, environment equivalence, blind SOVA-specific annotation, or
independent review.

### 5.6 Supported-runtime repetition

The research tests, integrated baseline, and no-Atlas vertical study were
executed in isolated environments on CPython 3.11.15, 3.12.13, 3.13.12, and
3.14.3. Volatile timestamps and performance measurements were excluded before
comparing the stable decision slices. All runtimes executed on one Windows
host.

### 5.7 Frozen Topic 12 public-contract study

After the Topic 12 implementation was complete, a separate protocol froze 12
observable contracts. Each contract was evaluated against eight deterministic
candidate traces: four equivalent-runtime variations, two material-outcome
variations, and two traces missing mandatory evidence. The expected per-trace
decision vector was fixed as four `reproduced`, two `not-reproduced`, and two
`inconclusive`. The study also required zero exact byte matches, zero
incomplete traces accepted as reproduced, and an aggregate result that did not
hide the missing-evidence trials.

The public implementation uses Wilson intervals and sensitivity strata. It is
not the same as the private Clopper-Pearson and partial-identification research
prototype. This study therefore validates an implemented conservative subset
without converting the private candidate into a public novelty claim.

## 6. Results

### 6.1 Classification

| Method | Macro-F1 |
|---|---:|
| Contracted mechanism | 1.000 |
| Exact trace | 0.111 |
| Exact final payload | 0.000 |
| Terminal status | 0.048 |
| Untyped event kinds | 0.048 |
| Remove context envelope | 0.778 |
| Remove evidence-validity gate | 0.778 |
| Collapse inconclusive into failure | 0.778 |
| Collapse inconclusive into success | 0.778 |

The contracted mechanism made zero false-equivalence decisions and zero false
divergence decisions on the frozen constructed corpus. Its full confusion
matrix is retained in the result artifact. Because this prototype emits
categorical decisions rather than probabilistic confidence scores, confidence
calibration is not applicable; a future learned or model-based judge must add
an independently labeled calibration study.

The perfect score is expected to be optimistic because the same team designed
the corpus transformations and the evaluator. It is evidence of correct
implementation against declared construction rules, not external validity.
The post-result ablations each misclassified the one balanced class whose
distinction they removed, reducing macro-F1 to 0.778. They were added under
protocol amendment 0.3 and are not represented as preregistered analyses.

### 6.2 Uncertainty gate and negative result

The initial preregistration required nominal 95% Wilson coverage. The seeded
simulation's aggregate coverage passed, but the stricter worst-cell
interpretation failed. Analytic Wilson coverage reached only 0.914 in the worst
predeclared `(p,n)` cell.

The original result was retained. Protocol amendment 0.2 added a two-sided
Clopper–Pearson interval computed by binomial-tail inversion. Minimum analytic
coverage over the same grid was 0.956. The exact interval is more conservative;
the statistical method is established prior art and is not a contribution.

### 6.3 No-Atlas vertical slice

All 12 cases and 24 traces passed the predeclared implementation checks.
Scripted and local executors used different mechanics and deliberately differed
in non-material stdout formatting, while the declared containment predicate
remained equal. Both traces and the derived capsule passed the independent
dependency-free verifier.

### 6.4 Integrated direct-baseline results

| Method | Correct / total | Accuracy |
|---|---:|---:|
| Integrated evidence-bounded mechanism | 7 / 7 | 1.000 |
| Public declared-observable exact comparator | 5 / 7 | 0.714 |
| Normalized request/outcome interaction equality | 3 / 7 | 0.429 |
| Terminal completion equality | 2 / 7 | 0.286 |
| AgentEvals 0.0.9 strict trajectory match | 2 / 7 | 0.286 |
| AgentEvals 0.0.9 unordered trajectory match | 2 / 7 | 0.286 |
| AgentEvals 0.0.9 superset trajectory match | 2 / 7 | 0.286 |
| AgentEvals 0.0.9 graph-strict trajectory match | 2 / 7 | 0.286 |
| Exact trace transport bytes | 1 / 7 | 0.143 |

The public comparator correctly handled ordinary observable equality,
divergence, and missing selected evidence, but it could not represent an
unsafe comparison or a required-context incompatibility. The integrated
mechanism also forced `inconclusive` when a complete trace lacked mandatory
claim evidence; before this test, behavioral predicates and fidelity results
were computed but not conservatively composed. That defect was corrected and
retained in the falsification record.

The normalized interaction comparator improved over completion and byte
equality by comparing request action/inputs and terminal status/output. It
still could not represent unsafe-to-attempt or incompatible contexts and
forced incomplete evidence into binary divergence. This is a controlled
request-key baseline inspired by transport replay, not a direct execution of
`agrepl` and not proof of transport isolation.

All four AgentEvals modes accepted the equivalent pair and repeat pair. They
also accepted structurally equal interactions under incompatible context, an
unsafe comparison, and a provider success contradicted by the SOVA oracle;
they called both missing-terminal/evidence cases divergent rather than
inconclusive. This does not show AgentEvals is defective for its intended
structural-matching use. It shows why structural trajectory matching alone is
not the same decision problem as evidence-bounded semantic reproduction.

On four pinned synthetic Inspect JSON-import cases, the candidate matched all four
constructed labels. Exact final-output equality matched one and terminal
completion equality matched two. All imported traces were explicitly marked
lossy. These tiny same-team cases demonstrate adapter implementability and a
counterexample to output equality; they do not establish performance against
Inspect, WebGraphEval, semantic metrics, or real evaluation logs. The later
thirteen-log Inspect corpus validates bounded format handling, while the
twenty-two-trajectory AgentRewardBench cohort supplies eleven retrospective
cross-model pairs with external labels. Neither is controlled re-execution or
purpose-built blind SOVA annotation.

Two independent offline implementations, one Python and one dependency-free
Node.js, agreed on package/content digests and required-key DSSE verification
for the same trace. This is cross-language engineering evidence, not an
independent scientific replication.

### 6.5 External deterministic baselines

| Method | Correct / total | False equivalence |
|---|---:|---:|
| Evidence-bounded candidate | 7 / 7 | 0 |
| Harness `ExactMatch` | 3 / 7 | 2 |
| Harness `ToolArgumentMatch` | 2 / 7 | 3 |
| Harness `ToolCorrectness` | 2 / 7 | 5 |

The Harness methods do not natively represent the incompatible, inconclusive,
or unsafe truth classes in this corpus. These results support the narrower
claim that binary structural matchers are insufficient for the frozen
multi-valued evidence problem. They do not show that Harness is defective for
its intended evaluation tasks or that SOVA is generally superior.

### 6.6 External real-model retrospective cohort

All twenty-two AgentRewardBench WebArena trajectories produced verified traces
and capsules, with zero known forbidden-content findings in the decoded
retained structures. Seven trajectories contained expert-label disagreement.
The eleven cross-model task pairs produced:

| Outcome | Count |
|---|---:|
| Equivalent | 2 |
| Divergent | 5 |
| Inconclusive | 4 |

The equivalent rate among seven conclusive comparisons was 2/7. Its two-sided
Wilson interval was 0.0822–0.6411; treating all four inconclusive cases as
unknown yielded a partial-identification interval of 0.1818–0.5455 over all
eleven pairs.

| Retrospective baseline | Exact agreement with SOVA decision |
|---|---:|
| Expert success-only | 7 / 11 |
| Exact action sequence | 6 / 11 |
| Action multiset | 6 / 11 |
| Terminal action | 2 / 11 |

The frequent inconclusive outcome is not a defect to be optimized away: it
records external expert disagreement rather than manufacturing equivalence.
The cohort is too small and selectively exposed in a demo Space to support a
population or state-of-the-art claim.

### 6.7 Cross-version decision stability

The integrated stable decision slice had digest
`sha256:891c07912f28e7743ea145593e73aa3d78c5e42ec10fc665cd11013408026960`
under every tested CPython minor version. The no-Atlas vertical decision slice
had digest
`sha256:f27cd1c4fdcff9903bf89e41de9436dc9e930b34ffc0347b16c8fd2f494d6f6f`
under every version. This is one-host cross-version evidence, not independent
cross-machine replication.

### 6.8 Graph and calibrated semantic-text baselines

The post-result baseline study pinned WebGraphEval's published construction as
the conceptual graph reference but deliberately did not reproduce its LLM
canonicalizer or necessity annotations. Instead, it built a deterministic
two-pass edit-similarity action graph with a 0.9 merge threshold. On the eleven
retrospective AgentRewardBench pairs it predicted every pair as divergent. It
therefore matched 5/7 conclusive labels and missed both equivalent pairs; it
also had no representation for the four inconclusive labels.

The semantic baseline used `model2vec==0.8.2` and the MIT-licensed
`minishlab/potion-base-8M` model pinned at
`bf8b056651a2c21b8d2565580b8569da283cab23`. Platt calibration used the STS
Benchmark train split and no test refit. On the held-out STS test split it
obtained accuracy 0.8216, Brier score 0.1267, ten-bin expected calibration error
0.0382, Pearson 0.7726, and Spearman 0.7541. With the explicitly post-result 0.5
trajectory threshold it matched 7/7 conclusive AgentRewardBench labels, but it
again forced all four inconclusive cases to divergent.

This is useful negative evidence: graph or embedding similarity can recognize
some material equivalence, but neither baseline carries authorization,
environment compatibility, evidence sufficiency, or an inconclusive state.
The semantic result is exploratory because calibration data are ordinary
sentence pairs rather than agent trajectories and threshold selection occurred
after the original P4 studies. No raw sentences, goals, URLs, reasoning, or
actions are retained in the result artifact.

### 6.9 Topic 12 public-contract result

All 12 contracts matched their complete expected decision vectors across 96
candidate traces. Decision accuracy was 1.000. The implementation accepted
zero incomplete traces as reproduced, and exact transport bytes matched zero
candidate traces. All 12 aggregate reports were `inconclusive`: each contained
four reproduced of six eligible trials plus two mandatory-evidence gaps. The
equivalent-runtime stratum reported 4/4 reproduced, the material-drift stratum
0/2, and the missing-evidence stratum zero eligible plus two inconclusive.

This is an intentionally adverse result for a headline success rate. It shows
that the implementation preserves uncertainty rather than silently dropping
incomplete trials. The fixtures and expected vectors remain same-team and
rule-derived, so the result is implementation evidence only.

## 7. Threats to validity

### Construct validity

The typed predicates may omit behavioral meaning important to a domain.
Passing them does not prove that the scenario author chose the correct
contract. The current corpus does not test open-ended semantic judges.

### Internal validity

The research team constructed both labels and implementation. The perfect
synthetic score may reflect specification alignment. The next study requires
independent labelers and hidden cases.

### External validity

Four CPython minor versions on one Windows reference host, deterministic
fixtures, thirteen Inspect logs, and twenty-two retrospective WebArena
trajectories do not represent controlled provider drift, fresh stochastic
trials, browser-state equivalence, distributed tools, or real incidents.
Hosted CI provides platform engineering evidence, not behavioral
generalization.

### Statistical conclusion validity

Synthetic repetitions do not justify population-level claims. Trial dependence
and environment effects require an explicitly fitted model in a real study.

### Security validity

`RestrictedLocalExecutor` is not a security sandbox. The study contains no
offensive payload and establishes no target security property.

## 8. Required experiments before submission

1. Freeze an independently authored corpus with hidden, purpose-built SOVA
   labels. The external AgentRewardBench labels are valuable but answer source
   success/side-effect/looping questions rather than the full SOVA contract.
2. **Completed as a bounded post-result baseline gate:** integrate at least
   `agrepl`, WebGraphEval or an equivalent graph baseline, and one calibrated
   semantic-text metric. The deterministic WebGraphEval-style graph adaptation
   and calibrated Model2Vec path satisfy the declared graph-or-transport
   alternative plus semantic-metric requirement. Direct `agrepl` execution
   remains desirable transport-isolation evidence, but it is not substituted
   with a fabricated implementation or claim.
3. Run controlled fresh trials across multiple model families, agent runtimes,
   tool protocols, operating systems, and version-drift conditions. The
   current real-model cohort is recorded retrospective evidence.
4. Validate deterministic and model-based judges against blinded human or
   ground-truth labels; report agreement, calibration, false equivalence, and
   false divergence.
5. Fit and diagnose a hierarchical model for item, model, runtime, and
   environment variation.
6. Ablate context equivalence, evidence sufficiency, predicate typing, and
   inconclusive handling.
7. Report cost, artifact size, capture overhead, privacy loss, unsupported
   cases, and negative results.

The offline external-review kit now enforces the minimum 120-case cohort,
all six profiles, seven required drift/failure strata, sealed-label and blinded
case digests, prediction freezing, exact case-set scoring, confusion matrices,
macro-F1, false-equivalence/divergence counts, and optional confidence
calibration. An independent author must still create and retain the cases and
labels; the tool cannot certify that human fact.

## 9. Current gate decision

- Paper mechanism: **GO for continued private research and this working
  manuscript**.
- External empirical status: **PASS-BOUNDED**.
- Submission or preprint: **HOLD** for blind labels, controlled independent
  re-execution, and independent adversarial review.
- Adoption or “industry standard” claim: **NO-GO**.
- Broad novelty claim for `.sova`, replay, or behavioral contracts:
  **NO-GO**.
- Narrow combined mechanism: **GO to private invention disclosure and counsel
  screening; patentability remains unknown**.

## References

1. Pattnayak and Bhatia. ReproEvalCard. ACL 2026.
   https://aclanthology.org/2026.acl-short.22/
2. Mudasiru. Deterministic Replay for AI Agent Systems. arXiv:2607.16200.
   https://arxiv.org/abs/2607.16200
3. Qian et al. WebGraphEval. NeurIPS 2025 workshop.
   https://openreview.net/forum?id=IPpOtWGmdf
4. NIST AI 800-2, Initial Public Draft.
   https://doi.org/10.6028/NIST.AI.800-2.ipd
5. Keller et al. NIST AI 800-3.
   https://doi.org/10.6028/NIST.AI.800-3
6. Lim et al. ContractEval. Findings of ACL 2026.
   https://aclanthology.org/2026.findings-acl.2112/
7. Wang and Huang. Security Is Relative. arXiv:2604.19012.
   https://arxiv.org/abs/2604.19012
8. Duy et al. PoC-Adapt. arXiv:2604.06618.
   https://arxiv.org/abs/2604.06618
9. W3C. Constraints of the PROV Data Model.
   https://www.w3.org/TR/prov-constraints/
10. OpenTelemetry. Semantic Conventions 1.43.0.
    https://opentelemetry.io/docs/specs/semconv/
11. LangChain. AgentEvals 0.0.9 deterministic trajectory matchers.
    https://github.com/langchain-ai/agentevals
12. Chan et al. AgentRewardBench: Evaluating Automatic Evaluations of
    Web Agent Trajectories. arXiv:2504.08942.
    https://arxiv.org/abs/2504.08942
13. UK AI Security Institute. Inspect AI evaluation logs and repository.
    https://inspect.aisi.org.uk/eval-logs.html
14. Harness. Harness Evals 0.15.0.
    https://github.com/harness/harness-evals
15. Ou et al. AgentDiagnose: An Open Toolkit for Diagnosing LLM Agent
    Trajectories. EMNLP 2025 System Demonstrations.
    https://aclanthology.org/2025.emnlp-demos.15/
16. MinishLab. Model2Vec 0.8.2 and `potion-base-8M`.
    https://github.com/MinishLab/model2vec
17. Cer et al. SemEval-2017 Task 1: Semantic Textual Similarity â€” Multilingual
    and Cross-lingual Focused Evaluation.
    https://aclanthology.org/S17-2001/
