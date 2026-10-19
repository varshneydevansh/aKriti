# Owned Indic CPU Reader and Selective Processing

**Date:** 2026-10-19  
**Status:** Planned and unrun; no reader implementation, training, model approval or measured improvement.  
**References:** [67], [74]-[75] in [the engineering reference index](akriti-research-reference-index.md).  
**Parent:** [CPU Text Reader and Local Compression](akriti-cpu-text-reader-and-local-compression-plan.md).

## 1. Decision and scope

Implement the useful routing, compact recognition and selective-processing ideas ourselves. Do not use the external OCR project's code, models, recognizer dictionaries, preprocessing, runtime, labels, outputs or verifier decisions. Ordinary numerical, image, font-rendering and tensor-execution primitives remain implementation plumbing, not an outsourced reader.

Any future license-compatible open-weight initialization requires separate approval and a manifest; none is selected here. The first research reader is an owned baseline, not a renamed outside engine or distilled copy of its outputs.

This is a CPU capability within the general-purpose VLM platform. Keep the existing aKriti Layout Reader, Text Reader, Table Reader, Chart Reader, Image Reader and Restoration Module boundaries. Kriti actions, native LibreOffice integration and Vinti workflow authority remain separate.

Start with printed Devanagari plus Latin text and shared numbers/punctuation. Broader Indic scripts, handwriting, mathematical recognition and very degraded sources are separate coverage gates. Supporting a script does not prove every language written in that script, every font or every document type.

## 2. What the updated reference changes

The 2026-10-19 external source snapshot supports studying routed compact readers and selective specialists more seriously. Earlier notes about six scanned-script families, no handwriting and a smaller package are historical observations, not current reference limitations.

Do not replace those old snapshots with a claim that handwriting, difficult Indic documents or a complete VLM are solved. Main-recognizer size, full model/data bundle, task-specific optional paths, CPU thread count and benchmark mode must remain separate measurements.

The named source assessment, publisher figures, licensing restrictions and unavailable/unverified claims are recorded in `LOCAL_DOCS/2026-10-19-indra-1-1-owned-reader-research.md`. Public engineering requirements below describe our proposed implementation, not performance inherited from that release.

## 3. Owned processing flow

```text
Original document and stable page render
    -> validate native text where present
          |
          +-> valid source-linked text -> preserve text and geometry
          |
          +-> absent, partial or conflicting text -> owned line/layout detection
                    -> non-destructive orientation/deskew and source transforms
                    -> script/mixed/unknown classification
                    -> owned shared CPU line-reader control
                    -> optional approved-by-policy specialist route
                    -> alternatives, scores and source-linked spans
                    -> deterministic critical-field/disagreement checks
                          |
                          +-> keep a supported reading
                          +-> one bounded reread with more visual context
                          +-> optional owned contextual or math specialist
                          +-> owned VLM if available within the device profile
                          +-> human review / abstain
    -> aKritiDoc source text, typed values, review and separate export views
```

No missing specialist, unsupported script or exhausted budget silently triggers a cloud service. A low-compute installation must still expose the original and a useful review state.

## 4. First owned recognition baseline

Candidate research architecture: a small convolutional visual stem, a compact sequence encoder, and a linear CTC head. This is a starting hypothesis for line recognition, not a requirement to reproduce a vendor architecture or hit its parameter count.

Begin with a declared Unicode label inventory, explicit blank/padding behavior and a no-language-model decoder. Preserve diacritics, script distinctions, original normalization form and immutable model output. Build labels from eligible source text, owned deterministic rendering or independently annotated real lines, never from external OCR outputs.

Font-rendered training examples require eligible fonts/text and can teach coverage, but rendered examples are not a replacement for real scan evaluation. Keep related fonts/templates/pages in the same split to avoid contamination.

CTC consumes a sequence of visual features and emits text units. It is not the same design decision as aKriti Core's language-model tokenizer. BPE alone is not an OCR architecture, and a word dictionary must not be necessary to preserve an unfamiliar name.

Track encoder output length against target length and repeated-label requirements. Width compression or a too-tight crop can make a target unrepresentable; that is not a decoder problem to fix with a more persuasive language model.

## 5. Indic representation and visual context

| Issue | Owned implementation requirement |
|---|---|
| Script versus language | Devanagari can represent Hindi, Marathi, Nepali and others; do not force language identity from script alone |
| Shared symbols | Digits, punctuation and blank lines can be script-ambiguous; allow shared/mixed/unknown outcomes |
| Short/long vowel signs | Distinguish signs such as U+093F and U+0940; preserve the consonant and neighboring visual context |
| Rendering versus logical order | A dependent vowel may render before its base while following it in logical text; do not assume one left-to-right glyph per output code point |
| Conjuncts, halant and nukta | Preserve marks and relationships; never treat all small components as noise |
| Unicode spans | Record code-point/byte offset convention, grapheme/orthographic segmentation profile and version, with mappings to source geometry |
| Mixed-script lines | Evaluate names, Latin abbreviations, native text and numbers together, not only artificially pure-script lines |
| Source identity | Keep raw extraction, model reading and normalized/evaluation forms distinct |

Reference [67] describes segmentation and permitted tailoring; it does not prescribe a recognizer or make glyph-to-token alignment trivial.

Under EXP-039, compare an explicitly declared code-point label scheme with a bounded composite/grapheme-aware alternative. Fix data, capacity budget, preprocessing and decoder policy. A whole-grapheme vocabulary can grow rapidly and lose unseen-combination coverage; it is not automatically better. Test reordering, unseen conjuncts, mark errors and decoding cost separately.

## 6. Script routing with abstention

Start with a shared Devanagari/Latin control. A router is justified only when fixed specialist candidates exist and the measured quality/resource tradeoff improves.

For trustworthy native text, script evidence may come from source character properties. For image-only input, any visual script classifier needs owned training/evaluation and its own error accounting. A predicted transcript is not independent evidence of the script it was already biased to produce.

Router outputs should include a bounded candidate set and `mixed`, `shared_symbols`, `non_text` and `unknown` where appropriate. A mixed line can retain a shared reader or use overlapping regions with preserved context; do not split at guessed script boundaries without evaluating clipping and merge errors.

On uncertain routes, compare a small bounded set of available owned readers or defer. Do not simply choose the largest raw confidence across heads. Token inventories, sequence lengths and calibration differ; comparisons need declared score semantics and held-out calibration. Keep raw scores and disagreement visible.

For EXP-038 freeze the readers and vary the routing policy only. Compare fixed shared routing, predicted routing and a labelled oracle-route diagnostic. Report route errors, downstream transcription errors, abstention, additional resident models and cold loading. Oracle routing is an upper bound, not the product result.

## 7. Selective rereading and contextual scoring

Use deterministic reasons first: clipped components, unsupported route, conflicting native/visual readings, critical numbers/names, invalid geometry, or low confidence under a declared policy. High confidence alone must not bypass critical-field review.

The first escalation should often be a better source crop rather than a bigger model: include ascenders/descenders, vowel marks, punctuation, neighboring characters and relevant line/table context. Preserve transforms and the original image. If line detection missed or merged content, rerun the appropriate layout stage rather than endlessly decoding the wrong crop.

Keep crop area, attempts, pixel/width budget, available models and elapsed work bounded. A proposed first policy allows one targeted reread before deferring; that count is an experiment setting, not an established optimum. Preserve initial and reread candidates. A repeated decode is not an independent vote.

An optional owned contextual scorer can rerank existing visual candidates after the no-language-model control works. Keep it separate from the visual score and record lexicon/corpus version, permitted document scope and whether the context was relevant. No grammar/dictionary prior may silently replace unsupported names, marks or amounts.

EXP-040 compares the same candidate set with no rescoring and one fixed contextual policy. Include correct, irrelevant and misleading vocabulary, unseen names and a no-candidate/unknown option. Report false substitutions/insertions and critical-field fidelity, not only average CER. Broader generation or vocabulary injection is a different experiment.

## 8. Math, handwriting and difficult documents

Math is a separate optional specialist path. First label formula-like lines versus numeric text, money, identifiers, tables and ordinary punctuation. Then compare a fixed available owned math reader with routing off, conditional routing and an all-eligible diagnostic. EXP-041 isolates the router; it does not train or adopt a math model automatically.

Require line/cell source mapping and retain literal alternatives. A formula-looking amount is not permission to rewrite a table. Score math fidelity and damage to ordinary cells independently, including the cost of a resident or cold-loaded specialist.

Handwriting is not promoted because a reference pipeline now attempts it. Collect and evaluate it separately by script, writer, ink quality and document condition. Until the owned path passes its gates, handwriting, faint text, stamps and degraded scans should expose uncertainty or defer to an available owned specialist/VLM/human.

Stamp-region detection is not content recognition or authenticity verification. Likewise, chart text recognition does not reconstruct chart values, axes or relationships. The full document VLM and structural readers retain their roles.

## 9. Bounded CPU execution

| Area | First owned strategy | Measurement |
|---|---|---|
| Scheduling | Bounded page queue; explicit interactive versus batch concurrency | UI responsiveness, throughput and peak memory |
| Line batching | Bucket similar widths and cap total pixels/feature lengths per batch | Padding cost, latency and memory by width |
| Models | Keep shared control available; cap resident specialist count | Warm/cold latency, reload churn and per-head bytes |
| Memory | Reuse bounded buffers; profile allocator/arena behavior rather than copying one toggle | Peak process memory, active/mapped pages, allocations and long-run growth |
| Threads | Compare a bounded thread pool with process-based alternatives when needed | Weight duplication, contention and cancellation |
| Precision | Float control first; INT8 only after quality/profile gates | Critical errors, runtime, activations and scratch buffers |
| Packaging | Declare all models, dictionaries, fonts/data, normalizers and runtime files | Total installation/download size, not just main recognizer weights |

Do not derive peak RAM from parameter count. Infrequently used resident specialists still consume memory; on-demand loading trades some residency for cold latency. Do not use every user CPU core by default in LibreOffice.

Native CPU integration is first. Browser CPU/WASM or optional GPU execution is later and needs its own operator, memory, threading and cancellation measurements. WebGPU is not the CPU path.

## 10. Chronological implementation plan

| Phase | Work and concept | Exit gate |
|---|---|---|
| C0: fixtures/contracts | Source IDs, transforms, script/critical-field labels, Unicode spans and independent expected readings | Reproducible permitted fixtures; no outside OCR labels |
| C1: native and shared control | Validated native text, owned line detection and no-LM Devanagari/Latin CTC reader | Unseen real-line results with preserved source geometry and unknown outcomes |
| C2: routing/units | Freeze readers; test script routing and label-unit choices separately | EXP-038/039 quality/resource evidence, including mixed and unknown scripts |
| C3: source reread | One bounded context-preserving crop reread | EXP-035 matched-risk quality, review and end-to-end cost |
| C4: optional specialists | Context scorer and math router as separate ablations | EXP-040/041 benefits without harmful rewrites or uncontrolled residency |
| C5: local runtime | Width buckets, bounded queues, float profile then one precision/allocation change | Stable long-document memory and critical-field preservation |
| C6: integration/coverage | aKritiDoc workbench/LibreOffice path; later additional scripts and handwriting | Source-linked review, cancellation and independently demonstrated coverage |

This extends EXP-027/028 and the existing CPU-reader baseline; it does not postpone aKriti schema, ingestion, exact search or workbench delivery. Models remain owned capabilities within the VLM-first architecture.

## 11. Controlled experiment cards

| Card | Hypothesis | Frozen control and promotion evidence |
|---|---|---|
| EXP-038-script-route-ablation | Script-aware routing helps at useful cost | Same reader checkpoints/candidates; route policy only; route errors, CER, unknown coverage and memory/cold latency |
| EXP-039-indic-label-units | A declared alternative text-unit scheme improves difficult marks/conjuncts | Fixed data/capacity/decoder policy; code-point and grapheme/critical-mark errors, unseen combinations and runtime |
| EXP-040-selective-context-rescore | Bounded contextual ranking helps without unsupported substitutions | Same visual candidates; no-LM control; positive/irrelevant/misleading context and critical-value accuracy |
| EXP-041-conditional-math-route | Selective math routing avoids unnecessary work and ordinary-cell damage | Same available owned specialist; route policy only; formulas, numeric-cell false routes, latency and residency |

Reuse EXP-035 for targeted rereads, EXP-036 for separately calibrated deferral and EXP-037 for broader context counterfactuals. Do not combine precision, vocabulary, crop, routing and model changes in one purported ablation.

An initial mechanics slice can use 50-100 eligible printed pages with document/font-family-separated splits and independently labelled lines/regions. Keep training, development, calibration and final evaluation separate. Include broken text layers, mixed scripts, shared digits, near-miss vowel marks, clipped crops, tables and unreadable regions. Pilot counts are not language-coverage proof.

Record raw code-point CER, declared normalized CER, grapheme errors, exact names/amounts/units, route/grounding mistakes, false acceptance, review load and unseen-script behavior. Keep image-only and native/hybrid modes separate. Report per-script denominators and intervals, not a pooled score presented as all-language success.

Declare one-device budgets before execution. A separately authorized 30-minute profiling/evaluation slice is a possible first step once fixtures and controls exist. Training, data collection and downloads need separate budgets/approval. No experiment has run.

## 12. Relationship to the wider plan

Use [Evidence and Adaptive Compute](akriti-evidence-and-adaptive-compute-research-plan.md) for typed decisions, correlated-reader caution, review and consent. Use [Confidence and Human Review](akriti-confidence-voting-review.md) for mandatory high-impact deferral. Use [Visual Retrieval Engineering](akriti-visual-retrieval-engineering-plan.md) to discover pages, not to certify their contents.

The intended advantage is measured Indic fidelity, bounded local execution and lower source-review burden, not a promised parameter count, language count, model adoption or benchmark win. This update introduces no external engine, weight approval, teacher material or product capability.
