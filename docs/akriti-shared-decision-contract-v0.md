# Shared Local Decision Contract v0

**Contract ID:** `local-decision/0.1-draft`  
**Date:** 2026-10-22  
**Status:** Owned documentation/design only. No executable schema, model, endpoint, calibration, migration or runtime integration is delivered.

## 1. Scope and authority

This contract shares a typed observation envelope, evidence references, calibration metadata and evaluation vocabulary. It does not select a common neural network, merge acoustic and visual verification, or grant application permissions.

Both repositories carry the same versioned design snapshot:

- aKriti: `docs/akriti-shared-decision-contract-v0.md`.
- Shruti: `docs/SHARED_DECISION_CONTRACT_V0.md`.

Propose changes together, revise the contract ID when semantics change, and update both snapshots before cross-project integration. Neither repository needs the other checkout to run. An executable shared package or synchronization mechanism is future work, not an existing dependency.

Existing `aKritiDoc`, `ShrutiTranscript`, module and action contracts remain authoritative. This draft attaches observations by reference; it does not silently migrate their schemas.

## 2. Separate three objects

```text
Bounded, permissioned source evidence
    -> modality-specific verifier
    -> DecisionObservation
    -> product-owned policy and current permission checks
    -> PolicyDecision
    -> separate correction/action proposal when needed
```

| Object | Responsibility | Must not imply |
|---|---|---|
| DecisionRequest | Bind one versioned task to available evidence and explicit candidates | Arbitrary questions are supported by every model |
| DecisionObservation | Record predictions, support assessments, uncertainty and failures | A proposed reading is true or an action is authorized |
| PolicyDecision | Apply versioned risk, budget, review and permission rules | Model confidence can override mandatory review |

False-positive/false-negative costs and action preferences belong to the policy, not to a redefinition of what the source contains. A budget may change whether we wait or review; it must not change the claimed truth of a reading.

## 3. DecisionRequest fields

| Field | Required semantics |
|---|---|
| `contract_id`, `request_id` | Exact contract revision and unique request identity |
| `task_id`, `task_revision`, `profile` | Registered task and evidence profile; unsupported tasks fail explicitly |
| `question` | Bounded task description; source content is data, never an instruction to execute tools |
| `candidate_set` | Ordered IDs, labels/value references, role, revision and content hash |
| `evidence_refs` | Source identities/revisions, availability, permissions and profile-specific locators |
| `context_refs` | Separately identified hints, provenance, scope and expiry; not replacement source evidence |
| `feature_provenance` | Encoder/checkpoint, frontend/tokenizer, candidate-generator and decoder revisions when used |
| `budget` | Deadline, memory/work/queue caps and permitted lookahead/retry limits |
| `policy_context_ref` | Versioned policy/risk context for the controller; not permission to change labels |
| `scope` | Local session/document/user scope and retention constraints; training eligibility defaults false |

One request asks one typed question. A future batch may share immutable state, but every observation remains independently attributable; batching is not evidence of independent errors or free inference.

The initial implementation should support registered boolean/choice tasks. General zero/few-shot natural-language questions and ordinal scoring are experimental capabilities, not v0 guarantees.

## 4. Candidate semantics

Use stable option IDs, not array indices as persistent identities. Hash ordered IDs, exact labels/value references, candidate revisions and task definitions; log the order used for each prediction.

For reading-selection tasks, distinguish:

- `candidate`: a concrete reading with an evidence-linked origin.
- `none_of_candidates`: the source supports a different reading; do not invent that reading in the selector.
- `insufficient_evidence`: available evidence cannot resolve the question, including unreadable/inaudible cases.

A profile defines mutually exclusive outcomes. Include the appropriate fallback outcomes rather than force a choice between two wrong candidates. Other task kinds need their own explicit outcome definitions.

Choice scores are conditional on the supplied evidence, question and option list. Test added distractors, candidate reordering, duplicate/paraphrased alternatives and missing-correct-candidate cases. Do not compare scores across changed option sets as if they were intrinsic probabilities of truth.

A learned evidence-sufficiency question may be evaluated separately, but it is another fallible prediction, not a proof. An ordinal level or its expected index is not automatically an interval-scale quantity, risk estimate or calibrated correctness probability.

## 5. DecisionObservation fields

| Field | Required semantics |
|---|---|
| `request_id`, `observation_id`, `contract_id` | Bind the result to the exact request |
| `status` | `scored`, `abstained`, `unsupported`, `invalid_evidence`, `stale`, `budget_exhausted`, `cancelled` or `failed` |
| `candidate_set_hash`, `source_revisions` | Exact inputs actually evaluated |
| `scores`, `score_kind` | Raw logits, normalized choice scores or explicitly scoped calibrated probabilities; null when unavailable |
| `selected_candidate_id` | Optional argmax/proposal, never automatic acceptance |
| `evidence_assessment` | Supported/insufficient/unknown assessment, method and uncertainty; source availability is checked separately |
| `support_refs` | References resolved from actual request evidence, with direct/derived/context-only/unavailable relationship |
| `reason_codes` | Bounded structured reasons, not free-form chain-of-thought or invented citations |
| `model_provenance` | Checkpoint/package hashes, model/head/feature versions, backend, precision and decoding configuration |
| `calibration` | ID, fit-data lineage, scope and `absent/in_scope/out_of_scope/invalidated` status |
| `runtime` | Capture/feature/model/gate costs where available, cold/cached state, elapsed time and complete memory scope |
| `dependency_refs` | Shared checkpoint, upstream output, transform, restored artifact and other correlated evidence |

A normalized categorical vector must have exactly the declared outcomes, finite nonnegative entries and a sum within the documented numerical tolerance. Validation of shape does not validate content. Unsupported/failed work must not manufacture a zero-confidence vector or a success-shaped result.

There is no mandatory `abstain_probability` field. Scorer abstention, an insufficient-evidence outcome, and a policy refusing automatic acceptance are different states. A learned abstention score needs its own labels, calibration and evaluation.

Calibration is conditional, not universal. Its scope includes task, language/script, source conditions, precision/runtime and candidate-generation policy where relevant. A changed scope cannot silently reuse an old correctness interpretation.

## 6. PolicyDecision fields and rules

Record `policy_id/revision`, input observation IDs, source/session revision, risk category, chosen thresholds, available budget, permission state, outcome and concise reason codes.

Possible outcomes include `keep_candidate`, `wait_for_audio`, `reread_region`, `replay_audio`, `request_review`, `abstain`, `reject` and `cancelled`. These are controller outcomes, not a universal shared-model label set.

Mandatory rules:

- Invalid, stale, unavailable or out-of-scope evidence cannot be made acceptable by a high score.
- Mandatory human-review and edit-approval rules remain in force.
- Exhausted work/lookahead budgets produce a bounded uncertain/review outcome, not forced commitment.
- Recheck source revision and session cancellation before consuming asynchronous results.
- A reading may be accepted for display while an application action still requires preview and confirmation.
- A correction is a new source-linked proposal/history event. Never overwrite original evidence or prior committed model output.

## 7. Separate evidence profiles

### SHR-ACOUSTIC-v0

The owned Shruti acoustic verifier consumes permitted audio/feature evidence and hypotheses. It is not a text-polishing model.

Record source audio/session revision, original-sample half-open interval, sample rate, retained/expired state, frontend/stride/crop mappings, causal input horizon, candidate/beam provenance, language/script hypotheses and context-vocabulary revision.

Acoustic scores, phonetic alternatives and a separately labelled critical-span detector may inform the observation. A plausible sentence, repeated checkpoint agreement or successful known-text alignment does not verify a spoken word.

Fixed-candidate ranking cannot recover an absent correct candidate. A bounded acoustic reread that creates new candidates is a separate operation with new provenance and cost. Expired or withheld audio must not be claimed as replayed.

Keep provisional, stable-for-display and committed transcript states separate from confidence and from action authorization. A compute-depth exit is not a transcript commit.

### AKR-VISUAL-v0

The owned aKriti visual verifier consumes original page/crop evidence, candidate readings and geometry. It does not receive speech as a substitute for pixels.

Record document/page/source revision, render identity, crop hash, bbox/coordinate profile, transform to original coordinates, neighboring line/glyph context, candidate origins and declared Unicode/segmentation/offset conventions.

Include vowel signs, halant/conjunct/nukta, mixed scripts, amounts/IDs, wrong-region candidates, unreadable text and missing-correct-candidate examples. A context-only language model cannot establish a missing stroke. Restored pixels are derived artifacts and may not settle a disagreement with the original silently.

Stamp detection, stamp-text reading, authenticity and downstream significance are different tasks. Signature presence is not identity or authenticity verification.

### TEXT-METADATA-v0 and WORKFLOW-v0

FilterTube may later supply bounded title/channel/metadata evidence; its explicit rules and playback authority stay product-owned. Semantic suggestions do not override deterministic policy.

Vinti may later consume grounded aKritiDoc facts and workflow state. It does not become an OCR engine or inherit a legal/workflow conclusion from an upstream confidence score. Unresolved source ambiguity and human-review requirements remain visible.

Separate models/heads are the default. Shared parameters are a later ablation requiring demonstrated positive transfer and no unacceptable per-domain regression.

## 8. Cross-project hints and source ownership

Document vocabulary may help speech recognition only within its permissioned document/session scope. Include tests where the document suggests A but the user says B. Audio determines the recognized utterance; pixels determine the document reading. The same fact appearing in dependent sources is not independent verification.

Kriti/Bolio may form an explicit proposed action from these artifacts. LibreOffice still owns selection validation, preview, native application and undo. The decision contract never executes a shell command, sends a message or edits a document.

## 9. Cache, lifecycle and privacy

Cache keys bind contract/task, evidence/profile/source revision, candidate order/hash, model/feature/precision/decoder, context, calibration, scope and policy revisions as appropriate. Prediction and policy caches are separate; a policy change need not imply a new acoustic reading but does invalidate cached action eligibility.

A cached-state benchmark does not establish low cold latency on changing audio or crops. Immutable-prefix reuse needs a defined state rule and measurement. Invalidate on relevant changes; reject stale late results after cancellation.

Bound cache entries, bytes, TTL, retained source horizon, pending requests and retry/lookahead work. Deletion invalidates replay availability. Hashes preserve identity, not the ability to reconstruct deleted evidence.

No silent cloud fallback, recording extension, private-data collection, cross-user/context leakage, online training or automatic reuse of corrections for global training. Ordinary storage/tensor/audio plumbing does not authorize external engines, verifiers, generated labels or teacher outputs.

## 10. Evaluation and promotion

Separate model fitting, policy/calibration selection and sealed evaluation. Group related pages/crops by document/source family, and audio chunks by recording/speaker/session. Do not expose final references or oracle choices to the deployed gate.

Report at least:

```text
selective_error = wrong automatically accepted items / automatically accepted evaluable items
coverage = automatically accepted evaluable items / all eligible evaluable items
```

Report critical-span variants, counts, exclusions, unreadable/inaudible cases, review burden and uncertainty intervals. Distinguish wrong-high-confidence/all-items from wrong/high-confidence-items. Zero accepted items gives undefined risk, not zero risk.

Include no-evidence, contradictory-context, candidate omission/reordering/distractor, language/source shift, precision change, stale revision, cancellation and budget-exhaustion cases. Record Brier/calibration diagnostics where labels support them, not only accuracy or average ECE.

Compare deterministic/simple heads, the strongest single verifier and optional packs at matched resources. Different roles still may have correlated errors. Count base, features, beam generation, gate, source I/O, rereads, cold loading, caches, merging and application costs.

Set device-specific caps and tolerances before an authorized run. Proposed percentage gains, 3-5% ECE or sub-1% error targets are hypotheses, not approved guarantees.

## 11. Build order and experimental boundary

1. Draft typed fixtures and contract mechanics alongside existing foundations; do not block Zero or document ingest.
2. Establish owned ASR and visual-reader controls separately.
3. Compare deterministic/logistic/small-MLP heads before a larger decision model.
4. Calibrate and evaluate within each supported scope, then test selective source recovery.
5. Investigate shared heads or a general Jev-like natural-language decision model only after task-specific controls exist.

A task-specific gate is not a general zero-shot reasoning model. A common API does not create common competence. Model-size targets, arbitrary-question support, distillation, ordinal scoring, shared weights and browser deployment remain experimental.

External implementations are research references, not dependencies, teachers, labelers or verifiers. Open weights need separate approval and lineage; approval of a paper, code licence or architecture does not approve weights or output-derived training material.

This document authorizes no execution, downloads, recording collection, paid compute, tests or training. Use the existing resumable Colab workflows only for separately authorized experiments; cloud success is not local deployment evidence.
