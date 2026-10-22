# aKriti Evidence and Adaptive-Compute Research Plan

**Date:** 2026-10-18  
**Status:** Proposed engineering/research, unimplemented and unmeasured.  
**References:** [67]-[73] in [the engineering reference index](akriti-research-reference-index.md).  
**Related contract:** [Confidence, Voting and Human Review](akriti-confidence-voting-review.md).

## 1. Objective and project boundary

Investigate fewer unsupported document corrections and lower review effort at a declared local memory/latency budget. Preserve the general-purpose document VLM identity and the owned aKritiDoc evidence contract. Vinti court workflows are downstream; Kriti owns separate action coordination; Shruti owns acoustic evidence and Bolio consumes speech capabilities.

The research opportunity is an evidence-directed system, not a claim that another model family or agent hierarchy is inherently novel. Publicly known quantization, recurrence, voting and classification methods do not create a moat by themselves.

Own the contracts, readers, decision policy, runtime and evaluations. External systems are references only, not engines, teachers, generated-label sources or verifiers. Approved open weights require separate manifests. This update does not approve artifacts or alter base-model selection.

## 2. Transfer from speech research: principles, not modality claims

The supplied speech report motivates separating source/model readings, typed semantic interpretation and presentation. It also motivates selective processing and probability-aware deferral. These are document-system hypotheses, not evidence that a speech architecture improves OCR or chart understanding.

| Speech-side concept | Document-side analogue | Important difference |
|---|---|---|
| Verbatim versus formatted transcript | Source-grounded extracted text versus canonical value/display form | Extraction remains a fallible hypothesis about pixels or a text layer |
| Audio span and competing words | Original page/crop and competing text/structure readings | Geometry, reading order and grapheme clusters matter |
| Context vocabulary | Document-local names, repeated values and lexicons | Context can bias a reader toward the wrong source value |
| Selective audio reprocessing | Targeted crop/line/table reread | Crops may need neighboring glyph, line or table context |
| Acoustic uncertainty gate | Owned structured document decision kernel | Scores must come from actual evidence, not text plausibility alone |
| Explicit correction history | aKritiDoc immutable provenance and review events | Human confirmation is authority, not just an additional model vote |

Speech-only synthetic-data, turn-taking or dynamic-depth results do not establish document quality, independent voter errors or safe local runtime.

## 3. Proposed evidence-preserving processing path

```text
Immutable original document and stable render
    -> owned layout/text/table/chart/image readers
    -> source-linked candidates and structural alternatives
    -> typed interpretation proposals
    -> deterministic policy / later compact learned gate
          |
          +-> keep a supported candidate under existing review policy
          +-> bounded targeted reread with source context
          +-> request human review or abstain
    -> separate normalized/export views
    -> Kriti/LibreOffice proposed action with its own approval
```

Retrieval discovers where to inspect; it does not verify the answer. Restoration produces a derived image; it does not replace the original. Repeated reads produce hypotheses; agreement is not source truth. Confidence is not action authority.

## 4. Critical-unit and script-aware evidence

Do not equate UTF-8 bytes, Unicode code points, model tokens, grapheme clusters, glyphs and visual regions. A Devanagari vowel sign may render before its consonant while following it in logical text order. A crop around only that mark can remove the evidence needed to identify the full unit.

Record the Unicode/segmentation profile and offset convention. Use grapheme-aware text spans with appropriate neighboring visual context; an orthographic syllable may require script-specific tailoring beyond a simplistic character splitter. Do not silently normalize away distinctions or mutate original text while preparing a model view. Reference [67] provides the segmentation framework, not an OCR algorithm.

For each correction proposal, preserve source document/page revision, render transform, source/crop hash, model/reader lineage, candidate text, affected span/structure, exact typed value if any, reason codes and review state. Map any derived crop coordinates back to the original source.

Stamps, seals and handwriting are separate evidence types. Detecting a stamp region does not verify its content, authenticity or legal effect. Language priors cannot reconstruct missing strokes with certainty.

## 5. A bounded typed decision kernel

Start with deterministic evidence checks and explicit limits. Consider a compact classifier only after there is a labelled failure taxonomy and a controlled comparison showing value.

Product-policy outcomes include `keep_candidate`, `reread_region`, `needs_text_review`, `needs_region_review`, `needs_precision_review`, `needs_human_review` and `abstain`. A modality-specific verifier first emits a separate support/uncertainty observation. Unknown/defer is supported; these actions are not a universal shared-model label set.

Inputs can include candidate disagreement, valid geometry, native-text validity, original-versus-derived relationships, type/units consistency, source completeness, critical-field flags, calibrated score features and remaining compute budget. The gate must not fabricate a character/value that no reader proposed with source evidence.

Avoid a single opaque confidence number. Differentiate raw model score, calibration estimate, deterministic check and human confirmation. Output concise structured reasons and references, not chain-of-thought.

Calibration/model/policy revisions and a candidate's source revision must travel with the decision. Reusing a decision on a changed document or render is invalid, even when the visible text looks similar.

## 6. Voting: complementarity must be measured

A role can have a bounded set of candidate readings; three/five/seven is a capacity choice, not a statistical guarantee. Do not require odd counts or run every voter for every region.

Readers sharing a checkpoint, tokenizer, training lineage, restoration or upstream parse can share failure modes. Repeated prompts and low-bit variants are not independent evidence. Measure pairwise error overlap and accepted-case errors on labelled fixtures, not variance across identical prompts.

An original crop, its restored derivative and a reread of that derivative belong to one dependency graph. Keep these relationships visible. Human review must see the original and alternatives; a human correction is a distinct accepted history event, not a model majority member.

Compare a deterministic gate, one targeted second read and any larger voter pack at matched budget. A pack is useful only if it reduces important errors or human work enough to justify its cost. A smaller learned gate cannot substitute for absent visual evidence.

## 7. Risk/coverage rather than decorative thresholds

The numerical examples in older confidence documentation are placeholders, not calibrated probabilities or approved acceptance thresholds. Fit a declared policy using calibration data separate from model/router training, then evaluate on a sealed test split.

Report coverage, abstention/review rate, false acceptance of critical errors, risk/coverage curves, per-script/task performance and uncertainty intervals. Few or zero observed errors on a tiny slice do not prove zero risk.

References [68]-[69] motivate risk-control research. Their mathematical guarantees require the actual assumptions, bounded loss and calibration procedure. Repeated crops from one document are not independent samples; domain shifts across fonts, scanners, languages and document families need separate tests. Do not advertise an unconditional per-document correctness guarantee.

Known high-impact review rules remain mandatory. Statistical acceptance of a reading does not authorize edits, court routing, signature validation or other downstream actions.

## 8. Selective compute with realistic state and memory

Begin with region-level rereads around fixed owned models. Compare against always-single-pass and always-second-pass controls. Use a held-out oracle selector only as a labelled diagnostic upper bound, not as the deployed gate or training truth.

```text
Expected cost = ingest/render + first pass + gate
              + selected rereads + crop/context preparation
              + model loading/I/O + state/replay + result merge
```

If a large specialist stays resident, its weight memory remains even when selected rarely. If it is loaded on demand, cold latency may dominate. Full-page context, overlapping crops and table/reading-order reconciliation can erase a naive span-level saving.

Measure difficult-case routing misses, including confidently wrong initial reads. Freeze a random audit subset of non-escalated regions during evaluation. Record compute by stage, cold/warm p50/p95 latency, process peak RAM/VRAM, review load and candidate correctness separately.

Do not start by combining recurrence, extreme quantization, multiple heads and a new router. Establish the original float path and one-variable ablations first. Speech early-exit reference [70] does not prove arbitrary document-model depth changes are valid.

## 9. Hard-case and cross-product counterfactual evaluation

| Slice | Required contrast |
|---|---|
| Indic script units | Vowel-sign/halant/conjunct/nukta alternatives with original context and declared segmentation |
| Critical values | Near-identical amounts, decimal marks, dates, units, IDs and negation |
| Tables/charts | The right text in the wrong cell/axis/series versus correct structural grounding |
| Stamps and degradation | Region present but content unreadable; restoration suggests unsupported strokes |
| Native text | A good digital layer versus broken font maps and a conflicting visible render |
| Contextual names | Correct dictionary, misleading near-neighbor and no context |
| Geometry/version | Changed crop/render/source version; candidate attached to the wrong page or stale source |
| Runtime precision | Float control versus changed precision that silently alters a critical value |
| Audio/document bridge | Document says A while the user deliberately dictates B; preserve the separate sources and proposed edit intent |

Keep text/span correctness, geometry, typed semantics and action correctness as separate labels. One exact match cannot establish all four.

A mechanics pilot can use 50-100 permitted pages with independently labelled queries/regions and document-family-separated splits. Augmented copies of the same page belong to one split. Public fixtures and an independently maintained holdout need explicit rights, consent, annotation provenance and leakage controls; sample counts alone do not establish coverage.

## 10. Planned experiment cards

| Card | Isolated question | Gate |
|---|---|---|
| EXP-034-critical-unit-evidence | Does source/typed-form separation improve important extraction outcomes over a formatting-only path? | Fewer exact-value/unit and forbidden-rewrite errors without worse grounding |
| EXP-035-budgeted-region-escalation | Does one owned targeted reread beat a fixed first pass at useful cost? | Matched-risk latency/memory/review improvement including gate, loading and crop cost |
| EXP-036-selective-risk-calibration | Does a calibrated defer policy improve accepted-case risk/coverage over fixed heuristics? | Sealed-test critical-error rates and intervals, language/task shifts and calibration validity |
| EXP-037-context-counterfactual | Does permitted context help without forcing unsupported names/values? | Helpful/irrelevant/misleading-context comparisons with false substitution and insertion counts |

All cards are planned and unrun. Begin only after source/schema fixtures and owned reader controls exist. A separately authorized 30-minute profiling/evaluation slice is a useful first budget; training, downloads and expanded data collection require their own authorization. No threshold, risk guarantee or device-fit promise is approved here.

## 11. Potential product advantage and learning loop

Potentially defensible assets are a rights-cleared real failure corpus, script-aware hard-case evaluation, predictable local runtime, lower correction burden and a well-integrated source-review experience. An open-source model can be part of this, but neither public architecture choices nor a model fork alone guarantees a commercial moat.

```text
Source-linked user correction
    -> local history and explicit personal preference, if requested
    -> optional separately consented research contribution
    -> failure taxonomy and eligible offline data
    -> fixed evaluation gate
    -> explicit released model/policy version
```

No silent online training, upload, private-document mining or cross-case dictionary leakage. Do not claim that deleting a dataset automatically removes its influence from a previously trained model. Permission for retention, personal adaptation and global training are different decisions.

Track user-facing measures such as time to correct a disputed amount, number of clicks to inspect the original, erroneous automatic changes and successful source-linked exports. Business differentiation requires repeatable user benefit and distribution/support, not only a benchmark chart.

## 12. Shared abstractions, separate products

Shruti's partner plan is `../../Shruti/docs/INDIC_EVIDENCE_AND_ADAPTIVE_COMPUTE_RESEARCH_PLAN.md`. Shared tooling may later reuse a decision envelope containing source references, alternatives, dependency lineage, calibration/policy IDs, budgets and reason codes. The audio and document contracts remain distinct.

Speech methods in [71]-[73] are cross-modality research references, not validated OCR implementations. No common neural backbone, automatic cross-modal truth vote, external engine dependency or immediate full-duplex feature is introduced.

## 2026-10-20 Clarification: Recoverable Evidence and Experimental Compute

Read [Selective Refinement and Recoverable Routing](akriti-selective-refinement-and-recoverable-routing-research-plan.md) and [Resumable Colab Experiments](akriti-colab-resumable-experiment-workflow.md). These supplements refine experiment sequencing, not the locked architecture or canonical schema.

- Source/schema mechanics and independent fixtures support parallel document-VLM and complementary CPU-reader controls. A complete OCR engine is not required before every VLM experiment.
- Keep fixed-candidate contextual ranking separate from source rereading that can generate a previously missing correct candidate. EXP-042 measures candidate coverage and recovery; an oracle candidate choice is diagnostic, not a deployed result.
- Audit non-escalated as well as escalated regions. Report confident critical errors, false corrections, risk/coverage, review burden and end-to-end cost.
- Recoverable visual-token deferral, EXP-043, is optional and architecture/schedule dependent. Include deferred representations, original positions, source I/O and gather/scatter in resource accounting. Preserve full-token controls and fine-mark/critical-value tests.
- Multimodal discrete-diffusion decoding, EXP-044, is separate from non-destructive image restoration. Neither fluent completion nor enhanced-looking pixels may replace the original source or settle ambiguity by themselves.
- Colab is optional training compute, not a GPU entitlement, runtime dependency or target-device result. EXP-045 specifies interruption/publication recovery only after an owned toy loop and explicit execution budget exist.
- Evidence retention is permissioned and bounded where appropriate. Preserving provenance does not require indefinite cloud copies or authorize training on private documents/corrections.

All new cards are planned and unrun. No schema migration, artifact approval, model download, external engine adoption, training, validation or benchmark is delivered by this supplement.

## 2026-10-22 Shared Contract and Separate Visual Research

Read [Shared Local Decision v0](akriti-shared-decision-contract-v0.md), the evaluation supplement and references [76]-[89]. Shruti carries a matching standalone snapshot and its twelve-project dictation research note.

Share requests/observations/policy semantics, evidence/version bookkeeping, failure vocabulary and calibration/evaluation tools. Original-pixel/geometry verification and acoustic/timing verification stay separate. Start with deterministic/logistic/small-MLP heads using owned representations.

Task-specific candidate support is not general zero/few-shot question answering. A common API creates neither common competence nor universal calibration. No shared backbone, 20-100M model, distillation or external teacher is selected.

| Card | Prerequisite and isolated comparison | Promotion evidence |
|---|---|---|
| EXP-046-decision-contract-mechanics | Existing source/schema fixtures; separate request/observation/policy fixtures | Invalid/stale/unsupported/unavailable states, stable option/evidence identities and unchanged authority |
| EXP-047-owned-visual-decision-head | Fixed owned visual reader/candidates and independent labels; deterministic versus simple head | Critical source support, unreadability and missing-candidate discrimination at useful coverage and full cost |
| EXP-048-decision-calibration-stress | Frozen head/candidate policy; disjoint fit/calibration/test | Script/source/precision risk/coverage, absent evidence, option-list robustness, intervals and review load |
| EXP-049-general-decision-transfer | Useful specialist controls first; optional shared/general model versus per-task heads | Held-out task-family transfer without unacceptable per-domain regression; full prefill/cached/cold memory and latency |

Add missing/unreadable/wrong-region sources, misleading context, candidate omission/reordering/distractors, precision and language shifts. A fallback class does not create reliable abstention automatically. An ordinal mean is not automatically a cardinal risk score.

Keep reading, typed interpretation, presentation and action separate. Selective correction, contradictory-context training and low-bit methods have prior art. Proposed percentage/ECE targets are hypotheses, not feasibility evidence or guarantees.

All cards are planned and unrun. A working document VLM or complementary CPU reader may supply the visual control; a complete OCR stack is not a gate for all VLM experiments. Optional Colab work retains permissions, actual hardware, budgets and full-state recovery; local deployment needs independent device evidence. No run, migration, model/artifact approval, download or training is delivered.
