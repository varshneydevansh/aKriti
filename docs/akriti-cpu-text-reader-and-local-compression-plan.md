# Owned CPU Text Reader and Local Compression Research Plan

Recorded: 2026-10-14.
Status: planned research; no implementation, checkpoint, benchmark, or base-model selection is established by this document.

## 1. Position in aKriti

aKriti remains a general document VLM/model-family platform. A compact CPU text reader is one independently implemented capability inside that platform, not a replacement for layout, tables, charts, images, grounding, or Kriti reasoning.

Public engineering references [50]-[61] are indexed in `akriti-research-reference-index.md`. Named source assessments are kept in `../LOCAL_DOCS/2026-10-14-indra-and-extreme-local-inference-research.md` to preserve vendor-neutral project architecture.

The research questions are:

- Can an owned compact line recognizer provide useful local text extraction before a more expensive document model is needed?
- Can source-aware routing avoid redundant visual inference without trusting a broken or unrelated PDF text layer?
- Can representation changes reduce measured device cost without changing source truth or confidence calibration?

Nothing here authorizes importing external OCR engines, external generated labels, external verifiers, or hosted services. Commodity tensor, image, PDF, and inference plumbing remains distinct from a third-party document-intelligence implementation. Any open-weight initialization needs separate approval, compatible licensing, and a recorded manifest.

## 2. Proposed owned flow

```text
input document
    -> owned ingest and immutable source identity
    -> inspect available text and geometry
    -> source-aware route
         -> validated native text extraction
         -> owned CPU layout/line reading
         -> owned VLM for visual or unresolved regions
    -> aKritiDoc with source regions and alternatives
    -> owned verification / human review where necessary
    -> explicit export or proposed document action
```

Native text is an optimization opportunity, not automatic ground truth. Evaluate missing glyph mappings, replacement characters, invisible or duplicate spans, implausible coverage, clipped boxes, image-only regions, and disagreement with rendered evidence. Language plausibility alone cannot validate a text layer.

Mixed pages must retain which spans came from native extraction and which came from visual reading. Deduplicate using geometry and source identity rather than silently choosing the more fluent string. Store the original reading before correction.

The fast reader must be able to return unsupported or uncertain. Handwriting, degraded scans, ambiguous stamps, complex tables, math, and tiny text should have explicit escalation paths. Do not discard a region simply because a cheap detector missed it.

## 3. Learning concepts and why they appear

| Component | Concept | Purpose and caution |
|---|---|---|
| Page/line features | convolution, subsampling, receptive fields | Turn local pixels into useful features; downsampling must not erase matras, nukta, or small digits. |
| Ordered line recognition | sequence modeling and CTC | Learn text without requiring a character-to-column annotation for every example. CTC collapse is not linguistic correction. |
| Targets | Unicode, grapheme clusters, vocabulary design | Distinguish encoded text from visible glyphs. Devanagari pre-base marks need learned logical reading, not naive pixel sorting. |
| Layout and order | geometry, spatial grouping, constrained ordering | Preserve columns, captions, table regions, and source coordinates. A line box is not a character-level proof. |
| Confidence | calibration and selective prediction | Determine when to abstain using verified source labels rather than raw softmax or repeated agreement. |
| Runtime | tensor lifetimes, SIMD, threading, caches | Optimize the bottleneck actually measured; many workers can multiply model copies and page buffers. |
| Compression | SVD, low rank, learned scales, binary factors | Explore alternative matrix representations; retained spectral energy is not a task-accuracy guarantee. |
| Training fit | frozen weights, autograd, checkpointing, offloading | Make a training step feasible without confusing host-memory movement with deployment compression. |

CTC is a potential sequence-learning bridge between document text and Shruti speech, not evidence that the two modalities can share one recognizer or one trained model.

## 4. Fixed-budget experiments

All cards are proposed and unrun. The initial 30-minute limits apply to bounded profiling/evaluation sessions after a working owned model exists, not to training an OCR model from scratch. Declare a separate training budget before training begins. Use train/dev material for selection and untouched ground truth for promotion.

### EXP-027-owned-cpu-line-reader

- Prerequisite: owned document ingest, line/text fixtures, aKritiDoc validation, and an independently trained compact reader with a reproducible manifest.
- Hypothesis: a compact CPU reader reduces end-to-end cost on clean supported print while preserving verified text and geometry.
- Slice: 24 labeled pages, six each of clean English, clean Devanagari, mixed print/numerals, and degraded or tiny-text stress cases. Unsupported slices remain explicitly unsupported.
- Controls: same page rendering, crop geometry, vocabulary, decoding, CPU threads, and output contract. No external OCR outputs are targets or validators.
- Budget: one device, 30 minutes, no automatic model downloads or cloud jobs.
- Metrics: Unicode/grapheme CER, exact names/dates/amounts, missing regions, line boxes, reading order, review recall, cold/warm latency, p95, peak RSS, and resident model/bitmap/workspace bytes.
- Promotion: meet tolerances written before the run; improvement must not rely on hiding difficult pages, dropping boxes, or omitting low-confidence fields.

### EXP-028-native-text-route-ablation

- Hypothesis: validated native extraction plus selective owned visual reading reduces unnecessary work without losing source content.
- Slice: 18 labeled pages, six each born-digital, scanned, and hybrid. Include deceptive or incomplete text layers in the born-digital/hybrid slices.
- Controls: compare visual-only, validated-native-only where applicable, and selective routing. Report each mode separately.
- Budget: 30 minutes on the same device and fixed rendering/worker policy.
- Metrics: routing mistakes, duplicated/missing spans, exact text/number errors, geometry, source provenance, latency and peak memory by page class.
- Stop: false acceptance of a bad text layer, unsupported content represented as read, or loss of original/corrected evidence history.

### EXP-029-risk-aware-low-bit-projection

- Prerequisite: frozen owned task model and an ordinary floating-point/INT8 comparison in the selected runtime.
- Hypothesis: changing one measured large linear projection's rank or precision improves memory/latency at an acceptable source-error cost.
- Order: low-rank floating point, scaled binary factors, recovery training, residual capacity, and latent rotation are separate ablations. INT4 and more extreme representations are later comparisons, not assumed winners.
- Slice: fixed labeled crops covering matras, nukta, conjuncts, digits, dates, names and ambiguity; independently verified labels are authoritative.
- Budget: 30-minute evaluation per variant; no large-model QAT or teacher/student job without an explicit budget.
- Metrics: matched-parameter and matched-compute controls, critical-field errors, grounding, calibration, abstention, actual operator dtypes, packed/resident bytes and temporary expansion.
- Stop: unrecovered task errors, false confident corrections, or a compressed export that expands the intended savings away.

### EXP-030-document-token-budget

- Hypothesis: reducing visual work on easy regions preserves fine-text evidence while decreasing a measured cost.
- Prerequisite: full-budget owned VLM baseline and source-region fixtures. Do not combine token reduction with a new quantizer in the first comparison.
- Slice: 24 labeled crops including tiny punctuation, decimals, matras, footnotes, table coordinates and stamp text.
- Budget: 30 minutes, one device, fixed decode settings.
- Metrics: retained source content, critical-field errors, grounding, page/crop latency, prefill/activation/cache memory and review behaviour.
- Stop: generic benchmark retention hides a lost detail or escalation lacks the original-resolution crop.

## 5. Runtime and claim accounting

Report these independently:

```text
unique trained parameters / serialized parameter entries
package bytes / installed bytes
resident weights / activations / caches / bitmap buffers / workspace
cold start / warm latency / end-to-end job time
single-page latency / sustained multi-page throughput
native-text / scanned / hybrid workloads
operator precision / checkpoint precision
```

Declare CPU model, usable cores, worker count, threads per worker, RAM, power mode, DPI, page dimensions, batch sizes, warmup, exclusions, and operating system. Measure a real Mac's chip/memory; do not inherit another publisher's server specifications.

Pixel and memory limits must produce explicit errors, tiling, or a documented lower-resolution path with retained original evidence. Unbounded worker multiplication and silent resolution loss are not acceptable optimizations.

Math and table interpretation are separately selected capabilities. A math path that improves a benchmark can still damage ordinary numeric cells. Keep raw cell text alongside any proposed formula or interpretation.

LibreOffice should call one bounded owned local service/library with shared lifetime-managed resources. A CPU reader can provide responsive selection assistance; the VLM and verified action layer remain available for harder work. CPU-first does not mean every device or every language is supported.

## 6. Promotion and confidence

Critical precision drift is measured against labeled source truth, not simply disagreement with a floating-point prediction. If the floating-point model is wrong and the compressed variant is correct, that is not a regression.

Measure rare-field and unsupported-script errors separately from aggregate CER. A fast wrong numeral or a mislocalized correction is a failure even when average text quality improves. Recalibrate confidence after quantization, token changes, routing changes or altered depth.

Repeated outputs from the same model, additional recurrent passes, and similar prompted voters are correlated evidence. They are not independent votes that automatically establish correctness.

Novelty, performance and license compatibility remain unestablished until their respective checks are completed. No research card changes the VLM-first identity, aKritiDoc contract, native LibreOffice integration boundary, or separate Vinti/Shruti product boundaries.

## 2026-10-19 Supplement: Indic CPU Readers and Selective Processing

See [Owned Indic CPU Reader and Selective Processing](akriti-indic-cpu-reader-and-selective-processing-plan.md), references [67], [74]-[75], and EXP-038 through EXP-041. This dated supplement updates the external source assessment without adopting its implementation.

Earlier six-script, unsupported-handwriting and smaller-package observations remain historical. The current reference describes a broader routed pipeline; that is not proof of reliable Indic handwriting, a one-model solution or an aKriti capability.

Owned build order: eligible source/Unicode/geometry fixtures -> validated native extraction and no-LM Devanagari/Latin line-reader control -> fixed-reader script routing and separate target-unit ablations -> one bounded source-context reread -> optional contextual ranking/math routing -> complete CPU memory/latency profile -> native integration and gated coverage expansion.

Preserve vowel signs, halant/conjunct/nukta relationships and logical/visual-order context. Script does not determine language. Mixed lines, shared digits, non-text and unknown scripts need explicit outcomes. Keep raw model text, typed values, normalized views and human corrections separate in aKritiDoc.

Do not select among reader heads by uncalibrated raw confidence, let a language prior override contradictory pixels, or treat repeated passes as independent votes. Reuse EXP-035/036/037 for bounded rereads, calibrated deferral and misleading-context checks. The new cards isolate script policy, label units, candidate rescoring and math routing.

Count every model, dictionary, rendering buffer, pending page/line batch, active or cached specialist and cold-load cost. Native CPU is the first deployment path; browser and other accelerators require separate validation. Handwriting and additional scripts are independently measured later coverage lanes.

All work remains planned and unrun. No external OCR code, weights, preprocessing, language models, dictionaries, generated labels, teacher outputs or verifier is adopted, and no benchmark result or model approval is created by this update.
