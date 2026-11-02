# `.sova-trace`: Claim-Conditioned Evidence Fidelity for AI-System Behavior Records

**Early working draft — not peer reviewed, not independently validated.** Published openly for transparency about research in progress; methods, results, and claims may change.

Version: 0.9, 2026-08-01  
Author: Gautam R. Patil  
Affiliation: TRUSCOR Private Limited  
Contact: gautam@truscor.org

## Abstract

Cryptographic integrity can show that covered trace bytes changed; it cannot
show that a recorder captured enough observable evidence to support a
particular AI-behavior conclusion. Capture profiles, omission, redaction,
conversion loss, clock uncertainty, missing causal edges, and unresolved trust
can leave a trace structurally valid while a claim is unsupported. We propose
a claim-conditioned Evidence Sufficiency and Fidelity Envelope for
`.sova-trace`. Each claim declares required event families, content visibility,
causal relationships, integrity, and trust assumptions. Evidence loss
propagates monotonically into `supported`, `weakened`, or `inconclusive` rather
than being hidden by a valid package or hash chain.

A preregistered same-team 120-case study matched all constructed support
labels; integrity-only, profile-name-only, and full-share baselines achieved
0.333, 0.167, and 0.167 accuracy. Component ablations, two dependency-free
verifiers, securesystemslib, rfc8785.py, and official OTLP protobuf exercised
chain, key-policy, canonicalization, and lossy-conversion boundaries. Thirteen
official Inspect AI logs first exposed an adapter defect (5/13 accepted); the
corrected adapter emitted 280/280 verified events while declaring every import
lossy. A retained privacy study demonstrated low-entropy dictionary recovery,
compromised-key recovery, linkability, exact ciphertext-length leakage, and a
secret-detector false negative; optional authenticated length buckets reduce
exact length to an explicit bucket but do not provide anonymity.
An explicitly lossy OASIS STIX 2.1 incident projection passed the official
`stix2` 3.0.2 parser and preserved source trace-digest and chain-root references.
An official `otelcol-contrib` v0.157.0 binary, verified against its published
Windows checksum, accepted eight local OTLP/HTTP spans and wrote all eight to
its file-exporter backend while preserving trace and span identifiers. Finally,
signed 1,000-, 10,000-, and 50,000-event traces passed indexed query,
payload-minimized disclosure, required-key verification, and independent
Python/Node digest agreement on one CPU host.

The mechanism is internally implementable and falsifiable, but the current
evidence is not an independent security or privacy evaluation. A frozen
external-review matrix now covers attacks and five interoperability families.
Submission remains on HOLD until an organizationally independent reviewer and
externally operated traces or production environments support or falsify the
claim; the new incident, Collector, and synthetic scale results are same-team
engineering evidence, not that external review.

## Extended evidence synopsis

Integrity verification answers whether covered bytes changed; it does not
answer whether a trace captured enough observable material to support a
particular conclusion. Capture profiles, redaction, omission, conversion,
clock uncertainty, missing causal edges, and unverified signer identity can
leave a trace structurally valid while making a behavioral claim unsupported.
This working paper proposes a claim-conditioned Evidence Sufficiency and
Fidelity Envelope for `.sova-trace`. Each conclusion declares its required
event families, content visibility, causal relationships, integrity, and trust
assumptions. Evidence transformations propagate monotonically into
`supported`, `weakened`, or `inconclusive` rather than being hidden by a valid
hash chain.

A preregistered 120-case synthetic study recovered all constructed
claim-support labels; integrity-only, profile-name-only, and full-share
baselines achieved 0.333, 0.167, and 0.167 accuracy. A small disclosure
prototype detected five specified mutations and disclosed the three-event
causal closure needed for one claim while withholding one unrelated event. A
separate reference-host substrate run recorded 10,000 compact events at 1,344
events/s and recovered the ten-event valid prefix of an interrupted trace.
An expanded post-result profile study found exact query fidelity on the
synthetic retained sets: Standard, Forensic, and Interpretability retained
8/8 events and 7/7 declared relationships, while Lite retained 6/8 events and
5/7 relationships, declared the two losses, and refused the memory-causal
conclusion while preserving the basic-behavior conclusion. Repeatedly adding
one identical artifact created one content-addressed blob. A partial trace
preserved two actors, two clock domains, explicit skew estimates, monotonic
order, and multi-segment verification.
In a separate 12-class synthetic adversarial matrix, corrupt, truncated,
reordered, substituted, privacy-leaking, malicious-archive,
parser-differential, signature-confusion, hostile-extension, and weak-key
cases were detected or rejected. Two negative controls were intentionally not
called detections: byte-identical replay and valid-signer equivocation both
remain individually valid offline and require trusted external freshness or
transparency state.
A post-result component ablation then showed that recomputing an unsigned
package manifest cannot substitute for event-level chain validation, that
required-evidence loss propagates to an inconclusive conclusion, and that
provider completion cannot substitute for a separately identified observer.
Two independently implemented offline verifiers—one Python and one Node.js,
neither importing the SOVA package—agreed on package and content digests,
validated required-key DSSE signatures, and rejected the tested chain and
key-substitution mutants. These ablations remain same-team tests rather than
an independent security or privacy review.
A thirteen-file pinned Inspect AI JSON corpus then exposed the
transcript-only assumption more broadly: adapter 0.2 accepted 5/13 and
typed-refused eight; adapter 0.3 handled three explicit official layouts and
emitted 280/280 verified events from the unchanged corpus, with every import
declared lossy and zero known forbidden-content probes. Twenty-two external
real-model AgentRewardBench trajectories produced verified
privacy-minimized traces/capsules without reasoning or screen data.
The official Inspect 0.3.251 Log API subsequently parsed and converted all ten
pinned public binary `.eval` fixtures. Adapter 0.3 initially accepted 9/10 and
typed-refused the message-deduplication fixture after it surfaced as log
version 3. Adapter 0.4 accepts version 3 only after the official reader expands
pooled event data and leaves `events_data` null; unexpanded pooled data still
fails closed. The unchanged cohort then yielded 483/483 verified SOVA events
from 358 source events and 31 incrementally read samples, with zero known
content-policy violations. This closes the narrow official-reader binary path,
not general Inspect compatibility or production adoption.
securesystemslib independently verified the SOVA DSSE envelope and rejected
five mutation/key controls; rfc8785.py agreed on 65/65 shared-domain
canonical values; and the official OTLP protobuf implementation preserved its
declared transport subset while making SOVA-specific losses explicit.
The official `openinference-instrumentation==0.1.56` OITracer, backed by
OpenTelemetry SDK 1.44.0, then emitted one nonzero valid-ID LLM span using
`openinference-semantic-conventions==0.1.30`. All six attribute constants used
by the bridge and all eleven span-kind values matched. This exposed and
corrected an invalid earlier `MEMORY` projection:
0.1.30 defines no such kind, so SOVA memory events now degrade explicitly to
`CHAIN`. The emitted content-bearing span lost its input and output under the
default import policy. The explicit preservation mode retained input/output
and labelled both as sensitive. The producer code is independently maintained, but the scenario,
harness, and interpretation are same-team; this is not an independently
operated producer or a privacy audit.
An explicitly lossy OASIS STIX 2.1 incident bundle was then accepted by the
official `stix2==3.0.2` parser. The import produced a seven-event verified SOVA
trace and preserved the custom source trace-digest and chain-root references,
while explicitly declaring that raw events, causal ordering, authorization,
redaction, replay semantics, and DSSE trust were omitted. Separately, the
official `otelcol-contrib` v0.157.0 Windows distribution was verified against
its published SHA-256 checksum and run as a local-only OTLP/HTTP-to-file
pipeline. It accepted eight protobuf spans and exported all eight with trace
IDs, span IDs, and required SOVA projection attributes preserved. No hosted
backend, account, credential, or external pipeline network was used.
A same-host scale envelope created signed traces of 1,000, 10,000, and 50,000
events. All three passed indexed queries, payload-minimized selective
disclosure, required-key verification, and independent Python/Node content-
digest agreement. The largest package was 6,183,508 bytes and contained fifty
event segments; traced peak memory was 54,973,724 bytes. Two complete runs
produced the same normalized semantic-invariant digest, while compressed byte
sizes varied by at most 3,563 bytes because each run generated fresh identities
and signatures. These are synthetic single-machine measurements, not a
production throughput benchmark.
These external implementation and corpus results strengthen interoperability,
but they do not show that the mechanism is novel, privacy-preserving against
realistic attackers, or superior to complete external systems. The broad
cryptographic ideas are prior art. Publication and patent filing remain held
pending independent privacy/security review, broader external traces, and
counsel review.

## 1. Problem

An AI behavior conclusion may require:

- a prompt and response;
- an observed tool action and effect;
- a parent relationship showing which request produced an outcome;
- a complete artifact;
- a named capture policy;
- valid package and event-chain integrity;
- a verified trust policy;
- explicit omissions and dropped-event counts.

A trace can be cryptographically intact yet lack one of those requirements.
Conversely, a redacted trace may still support a narrow claim that never
required the hidden content. A universal “valid trace” Boolean therefore
conflates transport integrity with claim support.

## 2. Proposed fidelity envelope

For each claim, the producer declares typed evidence requirements:

- required event kind and minimum count;
- whether full content is necessary;
- whether redaction is tolerable;
- mandatory or optional status;
- required causal parent kind;
- integrity and signer-trust assumptions.

Each requirement evaluates to:

- `present`;
- `redacted`;
- `omitted`;
- `missing`;
- `invalid`;
- `unverified`.

The claim becomes:

- `supported` when every mandatory requirement is present;
- `weakened` when only explicitly tolerated or optional requirements degrade;
- `inconclusive` when a mandatory requirement is absent, invalid,
  irreversibly redacted, or outside the supplied trust decision.

The central safety invariant is monotonicity: removing or invalidating required
evidence cannot increase claim support.

## 3. Completeness vocabulary

The design keeps separate:

1. **package continuity** — declared objects match their digests;
2. **event-chain continuity** — covered local events were not changed or
   reordered;
3. **recorder completeness** — the recorder reports whether it dropped data;
4. **sensor coverage** — the required source was instrumented;
5. **source completeness** — the observed producer emitted the relevant fact;
6. **semantic sufficiency** — the retained evidence can support this claim.

No hash chain proves the absence of unrecorded events. No signature proves that
an observation is true.

## 4. Disclosure graph prototype

A claim identifies seed evidence events. The prototype computes their causal
ancestor closure. Every event receives a domain-separated, salted commitment;
the ordered commitments are bound into a root. Events in the closure reveal
their bytes and nonce, while unrelated events retain only commitments.

The verifier checks:

- sequence and commitment binding;
- revealed event identifiers and bytes;
- the commitment root.

The root must itself be authenticated outside this prototype. Hidden
commitments do not prove that hidden plaintext is true, and withholding the
nonce prevents the receiver from testing guesses against hidden low-entropy
content. This is not claimed as a new commitment scheme, zero-knowledge proof,
or production cryptographic selective-disclosure protocol.

## 5. Prior art and standards boundary

OpenTelemetry supplies broad event and trace semantics. W3C PROV formalizes
provenance validity and equivalence. in-toto, DSSE, and Sigstore provide typed
attestation and portable verification material. Authenticated logs, Merkle
trees, salted redaction proofs, encrypted audit logs, and selective disclosure
have extensive paper, standards, and patent prior art. Inspect AI and MLflow
already preserve rich AI evaluation and tracing records.

The candidate research delta is not a new trace container or cryptographic
primitive. It is machine-checkable propagation from trace transformations and
capture loss into the support status of a declared AI-behavior conclusion.

## 6. Experimental protocol

### 6.1 Fidelity corpus

The synthetic corpus contains 120 claim/trace combinations with:

- complete evidence;
- redacted required content;
- omitted event families;
- invalid integrity;
- missing causal parents;
- unverified signer policy.

The tested claim requires a tool event linked to a prompt and an oracle event
linked to the tool.

### 6.2 Baselines

- integrity valid alone;
- capture profile name alone;
- full trace sharing without requirement analysis.

### 6.3 Disclosure mutations

The study mutates:

1. disclosed event content;
2. disclosed nonce;
3. commitment digest;
4. root;
5. sequence.

### 6.4 Substrate run

One CPU-only reference-host run measured:

- 10,000 compact events;
- 1,000 events with approximately 1 KiB model-response payloads;
- 100 identical artifact additions to exercise deduplication;
- valid-prefix recovery after an incomplete JSON tail.

### 6.5 Post-result component and independent-verifier ablations

The complete public trace substrate and private claim-fidelity mechanism were
then exercised under explicit removal or weakening of:

- the event hash chain after package descriptors and an unsigned manifest
  digest were recomputed;
- content-addressed blob integrity;
- trusted observer provenance;
- the integrity-to-claim fidelity gate;
- dependency closure during selective disclosure;
- Standard versus Lite capture;
- included-key versus required-key signature policy.

The same signed trace was independently checked by a Python implementation
that imports no SOVA module and by a dependency-free Node.js implementation.
Both support required-key DSSE/Ed25519 verification. Neither verifies external
timestamp/transparency state.

## 7. Results

### 7.1 Claim support

| Method | Accuracy |
|---|---:|
| Claim-conditioned fidelity | 1.000 |
| Integrity only | 0.333 |
| Capture profile name only | 0.167 |
| Full-share assumption | 0.167 |
| Remove causal-parent requirements | 0.833 |
| Remove full-content requirements | 0.833 |

No constructed case missing mandatory evidence was classified as supported.
As with the Topic 04 corpus, the same team wrote the transformations and
evaluator; the result shows specification consistency, not external validity.
The two component ablations were added after the original run under disclosed
protocol amendment 0.3. Each caused false support for the balanced case family
whose requirement it removed.

### 7.2 Disclosure prototype

All five specified mutations were detected. The dependency closure disclosed
three of four events (75%) and withheld the event explicitly labeled
irrelevant. This tiny case does not establish disclosure minimization,
unlinkability, dictionary resistance, or performance at scale.

### 7.3 Reference-host substrate

| Workload | Result |
|---|---:|
| 10,000 compact events | 1,185.15 events/s |
| Compact trace package | 728,688 bytes |
| 1,000 ~1 KiB responses | 1,152.03 events/s |
| Medium trace package | 78,132 bytes |
| Interrupted prefix recovered | 10 events |

The package sizes reflect ZIP compression and repetitive synthetic data.
Numbers are from one Windows/Python run and are not public performance claims.

### 7.4 Component-ablation and cross-language results

- A modified event segment was repackaged with fresh object descriptors and a
  fresh unsigned manifest digest. Package-only checks accepted it; full and
  independent verifiers rejected the stale event hash.
- Direct blob-byte substitution failed content-descriptor integrity.
- The executor conclusion was `conformant` with a trusted SOVA observer and
  `inconclusive` without it; provider status alone would have accepted it.
- Known invalid integrity made the claim `inconclusive`. An ablated evaluator
  that ignored the invalidity returned the false `supported` result.
- Dependency closure included both direct oracle parents; a seed-only
  disclosure omitted two required parents. The committed disclosure verified
  and rejected disclosed-content tampering.
- Standard capture supported the claim. Lite dropped the filesystem family
  and produced `inconclusive`, rather than relying on the profile name.
- Included-key verification remained
  `included-key-integrity-only`; a pinned key produced `required-key`; a wrong
  required key failed; carried timestamp material remained unverified.
- Python and Node.js independent verifiers agreed on the signed trace's
  canonical content digest and rejected the re-manifested chain mutant.

These results make the role of each layer concrete. They remain same-team
synthetic checks and are not a new cryptographic construction or privacy
proof.

### 7.5 Pinned external-fixture results

- At Inspect AI commit
  `1ea01a9e1b3c94db3cdf93ce5583ffebf86e159f`, thirteen official JSON logs
  contained transcript events, legacy flat events, no-event-stream records,
  and no-sample logs.
- Adapter 0.2 accepted 5/13 and typed-refused eight. This retained negative
  result falsified its transcript-only assumption.
- Adapter 0.3 accepted the unchanged 13/13 corpus, converted fourteen samples
  and 200 source events into 280/280 verified SOVA events, declared every
  conversion lossy, and produced zero known forbidden-content findings.
- All twenty-two cleaned AgentRewardBench WebArena trajectories produced
  verified traces and capsules using action verb/digest and external
  categorical labels. Goals, raw reasoning, arguments, screenshots,
  accessibility trees, URLs, chat messages, and annotator identity were
  excluded from retained decoded structures.
- The complete 1,408-row annotation table was summarized but not
  redistributed; the Space metadata did not assert a source-data licence.

### 7.6 Cross-library integrity and canonicalization

- securesystemslib 1.4.0 independently accepted the SOVA DSSE envelope and
  rejected payload mutation, signature mutation, a wrong key, key-ID mismatch,
  and semantically equal non-canonical payload substitution.
- rfc8785.py 0.1.4 produced byte-identical canonical JSON for 65/65 values in
  SOVA's intentionally shared domain. SOVA rejected a binary float that the
  full JCS implementation serialized; SOVA does not claim complete RFC 8785
  number support.
- These are cross-library byte/signature checks. They do not establish signer
  identity, key ownership, freshness, transparency inclusion, truth,
  completeness, non-repudiation, or unforgeability.

### 7.7 Official OTLP protobuf boundary

OpenTelemetry protobuf 1.44.0 preserved trace ID, span ID, parent, links, name,
start time, attributes, event count, and an unknown external attribute. The
round trip remained explicitly lossy:

- export omitted SOVA monotonic time, clock record, redactions, attempt, and
  previous-event hash;
- import could not reconstruct SOVA authorization, safety policy, absent
  monotonic ordering, capture-time-redaction proof, or the event hash chain.

No collector or hosted backend was used. The result establishes transport
compatibility, not semantic equivalence or production backend certification.

### 7.8 Quantitative privacy-attack study

A retained same-team study added explicit dictionary, linkage, length,
structure, compromised-key, nonce, and detector-negative-control cases:

- an unkeyed SHA-256 baseline uniquely recovered a low-entropy synthetic value
  from a 100-entry dictionary;
- a keyed commitment did not match any naive unkeyed dictionary candidate, but
  the same dictionary uniquely recovered the value after the HMAC key was
  treated as compromised;
- the same value produced equal commitments inside one context and different
  commitments across two contexts, confirming intentional within-context
  linkability and tested cross-context domain separation;
- unpadded AES-GCM ciphertext length had Pearson correlation `1.000000` with
  canonical plaintext byte length in the seven-length fixture;
- authenticated power-of-two padding reduced exact length to declared buckets,
  but bucket, placeholder, path, and surrounding-structure leakage remained;
- omission produced a constant-size marker over the fixture but still exposed
  presence, class, method, and path records;
- 512 encrypted repetitions produced 512 distinct nonces, while a generic
  sensitive string without a recognized secret shape remained a deliberate
  detector false negative.

These results caused an implementation change: encrypted placeholders can now
use authenticated ISO/IEC 7816-4-style power-of-two length buckets, and
decryption rejects padding metadata that differs from authenticated associated
data. The experiment does not prove HMAC, AES-GCM, anonymity, or malicious-
recorder resistance. It strengthens the policy decision that public export
defaults to omission.

These results improve schema, privacy, and interoperability falsification.
They do not demonstrate complete Inspect fidelity, general cross-format
equivalence, expert-judge calibration, or privacy against linkage and inference
from action digests.

### 7.9 Incident interchange, official Collector, and scale envelope

The incident path projected one safe eight-event SOVA trace into an OASIS STIX
2.1 bundle containing one each of `identity`, `incident`, `artifact`,
`observed-data`, `relationship`, and `note`. The official `stix2==3.0.2` parser,
pinned at commit `679bfe0befae5c39143cedcdc3cacf97ac31c890`, accepted the bundle. Import
created a seven-event trace that passed offline verification and retained custom
references to the source trace digest and chain root. Fidelity remained
explicitly lossy: STIX did not preserve the raw event stream, full causal graph,
authorization, redaction history, replay procedure, environment requirements,
or DSSE trust policy.

For the backend path, the official `otelcol-contrib` v0.157.0 Windows archive
matched the published checksum
`7b3938e1522ff04261a694a58e7111c5f7cdd19be617c3a77118cffad7abb815`.
The extracted binary had digest
`sha256:76146071912cc973553058795b7bb1b0c839991757257dabfce2d99fa89e26be`.
A local-only Collector configuration received eight OTLP/HTTP protobuf spans on
`127.0.0.1` and wrote eight records through the official file exporter. Trace
IDs, span IDs, and required SOVA projection attributes were preserved. This
establishes one real Collector path, not hosted-vendor certification,
production reliability, privacy, or semantic equivalence.

The scale envelope produced signed traces with 1,000, 10,000, and 50,000 events
and packages of 126,203, 1,238,352, and 6,183,508 bytes in the retained run.
Every case passed required-key verification, indexed queries, 100-event
payload-minimized disclosure, and dependency-free Python/Node content-digest
agreement. The 50,000-event case used fifty segments, captured at 325.6 events/s
under instrumentation, and reached 54,973,724 bytes of traced peak memory.
Those timings are diagnostic one-host samples, not product performance claims.
The largest case is 56.88 times the 879 events retained across the two external
corpora, but it is synthetic and does not substitute for independently operated
external-corpus scale.

## 8. Security analysis

The prototype and public trace foundation are tested against malformed
archives, traversal, duplicate names, excessive compression, event insertion,
deletion, reordering, substitution, duplicate identifiers, forward causal
parents, invalid signatures, signer-policy mismatch, malformed redaction
placeholders, secret-shaped fields, and truncated tails.

Residual threats include:

- a dishonest or compromised recorder;
- an uninstrumented source;
- colluding signer and recorder;
- semantic omissions not declared by the producer;
- correlation, bucket-size, path, and surrounding-structure leakage;
- low-entropy guessing if salts or keys are exposed;
- malicious but schema-valid extension payloads;
- unverifiable external timestamps or transparency data;
- claims whose evidence requirements were authored incorrectly.

## 9. Threats to validity

### Construct validity

The envelope evaluates author-declared evidence requirements. An incorrect or
incomplete claim specification can produce a mechanically correct but
scientifically misleading conclusion. Event presence is not observation truth,
and a trace may omit an uninstrumented effect without detectable discontinuity.

### Internal validity

The SOVA team authored the mechanism, constructed corpus, most converters, and
interpretation. Perfect synthetic labels primarily show executable-specification
consistency. Independent libraries and external corpora reduce implementation
coupling but do not remove same-team selection or analysis bias.

### External validity

The current external corpus is bounded to Inspect JSON and `.eval`, privacy-
minimized AgentRewardBench trajectories, canonical/signature libraries, one
OASIS STIX incident projection, one local official Collector/file-exporter path,
and an OTLP protobuf round trip. It excludes hosted production backends,
diverse vendor traces, real incident corpora, malicious recorders, and
independently operated environments.

### Statistical validity

The 120 cases are deterministic and balanced by construction. Reference-host
throughput and size measurements, including the 50,000-event synthetic envelope,
have no multi-machine variance estimate. The external study must report
denominators, per-stratum uncertainty, and failures rather than aggregate
success percentages alone.

### Security and privacy validity

The same-team attack matrix is not a cryptographic audit. Signatures and hash
chains provide bounded tamper evidence, not truth, freshness, completeness,
non-repudiation, or signer honesty. Authenticated padding still leaks buckets,
paths, presence, and surrounding structure; omission remains the safest public
default for secrets.

## 10. Required work before submission

The private external-review tool now freezes the complete P5 attack and
interoperability case matrix, requires coverage of the five declared source
format families, validates one reviewer finding per case, and preserves every
adverse disposition. This prepares a reproducible handoff; it cannot create or
authenticate an independent reviewer.

1. Freeze independently authored claim/trace cases.
2. **Completed as a bounded same-team interoperability gate:** expand the
   Inspect, AgentRewardBench, and OpenInference studies to an incident format
   and a production-grade Collector code path. The official STIX parser and
   official `otelcol-contrib` binary now cover one explicitly lossy incident
   projection and one local Collector/file-exporter backend. Hosted services,
   production operations, and independently produced incident corpora remain
   external-validation work.
3. Expand the completed synthetic four-profile tests to externally produced
   traces and claims.
4. Have an independent reviewer repeat and extend the completed same-team
   dictionary, correlation, length, structure, equivocation, replay, parser
   differential, signature-confusion, and conversion-loss cases.
5. Compare with authenticated-log and established selective-disclosure
   constructions rather than the illustrative root.
6. **Completed for a synthetic size envelope:** measure indexed access,
   streaming/finalization time, traced memory, package size, disclosure
   minimization, and both independent verifiers through 50,000 events. Repeat
   at independently operated external-corpus scale before submission.
7. Obtain a cryptographic review and privacy threat-model review.

The offline reviewer packet and deterministic P4 blind-release/scoring tool are
prepared. Tooling validates digests, completeness, and release order but does
not manufacture reviewer identity or independence.

## 11. Current gate decision

- External empirical status: **PASS-BOUNDED**.
- Paper: **GO for this private working manuscript; HOLD for submission until
  independent privacy/security review and independently operated external
  traces or environments**.
- Broad patent on traces, commitments, redaction, or selective disclosure:
  **NO-GO**.
- Narrow claim-conditioned fidelity propagation: **GO to private invention
  disclosure and counsel screening; patentability unknown**.
- Public claim of “unforgeable,” “non-repudiable,” “complete,” or “true”
  evidence: **NO-GO**.

## References

1. OpenTelemetry Semantic Conventions 1.43.0.
   https://opentelemetry.io/docs/specs/semconv/
2. W3C PROV-CONSTRAINTS.
   https://www.w3.org/TR/prov-constraints/
3. W3C PROV-AQ.
   https://www.w3.org/TR/prov-aq/
4. DSSE protocol 1.0.2.
   https://github.com/secure-systems-lab/dsse/blob/master/protocol.md
5. Sigstore Bundle Format 0.3.2.
   https://docs.sigstore.dev/about/bundle/
6. Inspect AI evaluation logs.
   https://inspect.aisi.org.uk/eval-logs.html
7. NIST AI 800-2, Initial Public Draft.
   https://doi.org/10.6028/NIST.AI.800-2.ipd
8. US11032065B2, verifiable redactable audit log.
   https://patents.google.com/patent/US11032065B2/en
9. US11886557B1, auditable Merkle selective disclosure.
   https://patents.google.com/patent/US11886557B1/en
10. Chan et al. AgentRewardBench: Evaluating Automatic Evaluations of
    Web Agent Trajectories. arXiv:2504.08942.
    https://arxiv.org/abs/2504.08942
11. securesystemslib 1.4.0.
    https://github.com/secure-systems-lab/securesystemslib
12. RFC 8785, JSON Canonicalization Scheme.
    https://www.rfc-editor.org/rfc/rfc8785
13. OpenTelemetry protocol and protobuf definitions.
    https://github.com/open-telemetry/opentelemetry-proto
14. UK AI Security Institute. Inspect AI Log File API and `.eval` format.
    https://inspect.aisi.org.uk/eval-logs.html
15. OpenInference semantic conventions and instrumentation.
    https://github.com/Arize-ai/openinference
16. OASIS. STIX Version 2.1.
    https://docs.oasis-open.org/cti/stix/v2.1/os/stix-v2.1-os.html
17. OASIS Open. STIX 2 Incident Core Extension.
    https://github.com/oasis-open/cti-stix-common-objects/tree/main/extension-definition-specifications/incident-core
18. OASIS Open. `cti-python-stix2` 3.0.2.
    https://github.com/oasis-open/cti-python-stix2
19. OpenTelemetry Collector Contrib v0.157.0.
    https://github.com/open-telemetry/opentelemetry-collector-releases/releases/tag/v0.157.0
20. OpenTelemetry Collector file exporter.
    https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/exporter/fileexporter
