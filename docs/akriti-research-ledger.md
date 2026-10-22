# aKriti Research Ledger

**Status:** Living reference, snapshot locked on 2026-05-19  
**Purpose:** Preserve the research discussion from `LOCAL_DOCS`, provider-native code agent harness chats, and deep-research reports so future work does not re-litigate settled architecture.

## 1. Ledger rules

Each item is classified as:
- `adopt-now`
- `v1.5-experiment`
- `v2-training`
- `v3-research`
- `reference-only`
- `do-not-depend`

Product boundary:

```text
Use public projects for ideas, evaluation, and open-weight initialization.
Do not make external OCR/VLM systems final product dependencies.
```

## 2. Current open-weight base candidates

| Item | Classification | Use in aKriti |
|---|---|---|
| open-weight base-family candidate | `v1.5-experiment` | strongest current open-weight open-weight base candidate family for document/VLM work |
| open-weight base-family candidate | `watch` | pending near-term candidate; evaluate when open weights and docs stabilize |
| open-weight edge-model reference / open-weight edge-model reference edge family | `v1.5-experiment` | local/mobile/tiny model reference, LiteRT path, runtime and behavior comparisons |
| external OCR specialist | `reference-only` / `engineering-reference` | layout-first OCR ideas, structured outputs, compact specialist design, MTP-style speed ideas |
| external OCR specialist / OCR2 | `reference-only` / `engineering-reference` | token budgeting, multi-resolution/foveation, reading-order ideas, prompt modes |
| external OCR specialist | `reference-only` / `engineering-reference` | independent OCR/document bake-off candidate |
| external OCR/layout toolkit/layout tools | `reference-only` / `baseline-only` | deterministic OCR/layout baselines and error-type comparison |

Final shipped product must use aKriti-owned modules/checkpoints, not these external engines.

## 3. Document intelligence and UI references

| Item | Classification | Use |
|---|---|---|
| external document-layout reference PDF layout analysis | `adopt-now as UX/API reference` | visual overlays, block classes, reading-order UI, layout inspector |
| Datalab / Marker / Chandra-style outputs | `reference-only` | Markdown/HTML/JSON/chunks product contract |
| Sarvam Akshar | `reference-only` | grounded document workbench inspiration, Indic-first UX warning |
| VLM Run Orion | `reference-only` | plan-execute-reflect, tool-augmented visual intelligence, visual judges |
| OpenDataLoader PDF | `reference-only` | born-digital PDF deterministic fast path concept |

## 4. Retrieval and document QA

| Item | Classification | Use |
|---|---|---|
| "Is Grep All You Need?" | `adopt-now` | exact search first, vector second; crucial for dates/names/citations/tables |
| DeepDive | `v2-training` | KG-style synthetic QA and multi-hop search for Vinti/court workflows |
| Long-context RAG papers | `reference-only` | support retrieval-first design; avoid dumping entire documents blindly |

Locked retrieval policy:

```text
exact search -> layout/entity search -> metadata filters -> vector search -> long-context reasoning
```

## 5. Training, fine-tuning, and adapters

| Item | Classification | Use |
|---|---|---|
| Hugging Face PEFT | `adopt-now` | practical LoRA/QLoRA/adapter experiments |
| LoRA / QLoRA / DoRA | `adopt-now` | first adaptation path over open weights |
| LoRA-FA / VeRA / Delta-LoRA / LoRA+ | `v1.5-experiment` | compare for adapter efficiency |
| adaptive low-rank training reference | `v1.5-experiment` / `v2-training` | adaptive per-module rank discovery |
| Unsloth | `v1.5-experiment` | practical adapter training acceleration where compatible |
| Fine-tuning guide PDF | `reference-only` | broad training lifecycle reference, not latest strategy source |
| Distribution Fine-Tuning | `v2-training` | natural writing, translation style, anti-AI-slop document rewriting |
| SU-01 / verifiable reasoning recipes | `v2-training` | self-checking and RL with verifiable rewards |
| RLVR / unit-test rewards | `v2-training` | table/layout/chart/schema correctness rewards |

## 6. Training efficiency research

| Item | Classification | Use |
|---|---|---|
| token-superposition-style pretraining reference | `v2-training` | pretraining/continued-pretraining speedup while final model stays standard |
| hierarchical attention reference | `v3-research` | long-context pretraining cost reduction |
| neuron-preserving optimizer optimizer | `v2-training` | optimizer candidate to avoid neuron death/capacity loss |
| four-bit pretraining reference pretraining | `v3-research` | Blackwell/H200-class low-precision cloud training |
| tile-wise sparse FFN | `v3-research` | GPU research source sparse activation/inference/training lane |
| Attention Residuals | `v2-training` | decoder stability/efficiency experiment |

## 7. Runtime and deployment

| Item | Classification | Use |
|---|---|---|
| llama.cpp / GGUF | `adopt-now` | CPU/GPU research source local inference path |
| extreme quantization reference | `v1.5-experiment` | KV cache compression for long-document local inference |
| MLX | `adopt-now` | Apple Silicon local model lane |
| ONNX Runtime | `adopt-now` | cross-platform and browser-adjacent deployment |
| WebGPU/WASM | `adopt-now` | FilterTube and browser-local experiments |
| LiteRT / LiteRT-LM | `v1.5-experiment` | Android/iOS/edge deployment and skill/session patterns |
| Core ML | `v1.5-experiment` | Apple-native/mobile runtime |
| vLLM / SGLang | `adopt-now for server` | teacher/batch/cloud serving |
| CUDA/TensorRT | `v2/v3` | GPU research source workstation/cloud optimization |

## 8. Visual encoders and world models

| Item | Classification | Use |
|---|---|---|
| world-model reference / world-model reference | `v1.5-experiment` | small visual representation experiments, page/thumbnail embeddings |
| V-world-model reference 2/2.1 | `v1.5-experiment` | frozen video/keyframe/thumbnail embeddings |
| DINOv2 | `baseline` | visual embedding baseline |
| SigLIP / CLIP | `baseline` | page/image/thumbnail semantic embedding baseline |

Use for:
- FilterTube thumbnails/keyframes
- document page-region embeddings
- layout anomaly detection
- chart/table/image triage

Do not use as primary OCR or reasoning model.

## 9. Restoration and diffusion

| Item | Classification | Use |
|---|---|---|
| CURIO-like rectification | `adopt-now as idea` | deterministic/learned curvature/dewarp lane |
| TAIR | `reference-only` | text-faithful restoration risk model and eval inspiration |
| LucidFlux | `v3-research` | universal restoration idea, risky for factual text |
| Diffusion LMs / dLLM | `v3-research` | infilling/editing research only |
| Mercury-style diffusion LLMs | `do-not-depend` | remote/closed or not core; not primary parser |

Locked rule:

```text
diffusion is restoration-only for now;
autoregressive transformer-family VLM remains core parser/action model.
```

## 10. Interpretability and audits

| Item | Classification | Use |
|---|---|---|
| Goodfire neural geometry/Fourier calculator | `v2-research` | probe whether model learns page/table/date/chart geometries |
| interpretability research reference Natural Language Autoencoders | `v3-research` | activation explanation/audit for Kriti later |
| calibration/evidence audits | `adopt-now` | confidence and provenance evaluation |

## 11. Voice and accessibility

| Item | Classification | Use |
|---|---|---|
| Supertone Supertonic 3 | `optional add-on` | local TTS/read-aloud/accessibility if license fits |
| ASR/tiny speech models | `future` | voice commands for LibreOffice/mobile |

Not core to the first VLM parser.

## 12. Product-specific ledgers

### LibreOffice

Adopt:
- C++ native sidebar/canvas
- local service/embedded runtime bridge
- selection-grounded actions
- preview/apply/rollback edits

Avoid:
- WebView shell as core LibreOffice integration
- third-party cloud dependency

### FilterTube

Adopt:
- rules first
- tiny text classifier
- thumbnail vision classifier
- tiny VLM only when ambiguous
- WebGPU/WASM/ONNX runtime
- cache by video id and thumbnail hash

### Vinti/court

Adopt later:
- aKritiDoc evidence graph
- exact search/provenance first
- chronology/entity/deadline extraction
- court-specific synthetic/evaluation data

Boundary:

```text
assist procedure and structure;
do not judge or issue verdicts.
```

## 13. Final research synthesis

All research maps into six lanes:

```text
1. document schema and UI
2. owned model training/adaptation
3. runtime/local optimization
4. restoration and visual preprocessing
5. verification/provenance/evaluation
6. product integrations
```

Anything that does not improve quality, local compute, reliability, or ownership remains research-only.

## 14. Research-to-experiment control

See `docs/akriti-research-to-experiment-matrix.md` for the operational rule that every new paper, model, optimizer, OCR/VLM reference, runtime idea, diffusion/restoration idea, or UI concept must map to:

```text
classification -> owned module lane -> benchmark slice -> fixed-budget experiment -> promotion gate
```

This prevents roadmap drift while still letting aKriti absorb useful research aggressively.

## External Research Use Lock

The project policy is now stricter than a normal research ledger: external systems are not teachers, verifiers, labelers, runtime services, or product components. They are engineering references only. The only external artifact allowed into the model lineage is open weights, and only after recording license, version, role, and provenance in the model manifest.

Detailed named research notes are kept outside the project repo.

## Research References

This doc is connected to the numbered research bibliography in `docs/akriti-research-reference-index.md`. Those references are engineering anchors for aKriti-owned implementation; they are not product dependencies. Only open weights may enter model lineage, and only with manifest provenance.

## 2026-05-21 Training/Restoration Update

| Engineering reference | Classification | aKriti interpretation |
|---|---|---|
| periodic low-rank merge training [24] | `prototype later` | More valuable for `aKriti Core` than restoration-specific personalization because it can reduce memory pressure during continued pretraining/domain adaptation. |
| continual low-rank visual personalization [25] | `watch / later prototype` | Useful later for degraded document restoration, especially stamps, seals, old scans, handwriting styles, blur, and Indic glyph repair. |
| large-cluster inference network topology | `drop for now` | Not relevant to local-first LibreOffice, browser/WebGPU, RTX 2060, Mac, or first Core model work. Revisit only for owned hosted inference clusters. |

## 2026-05-28 Document Grounding and Precision Update

| Engineering reference | Classification | aKriti interpretation |
|---|---|---|
| structured OCR-layout-table output schema [30] | `adopt-now as schema/eval target` | aKritiDoc must preserve labels, reading order, HTML/table/math candidates, polygons, bboxes, confidence, skipped/error states, and page bounds. External systems are benchmark/reference only. |
| parallel visual grounding and query-to-region localization [31] | `adopt-now as owned module interface; prototype later` | Add `aKriti Grounding Module` so claims can locate supporting regions: seals, signatures, paragraphs, case numbers, tables, handwritten notes, formulas, and low-confidence spans. External grounding weights are not product dependencies. |
| precision-stability testing for low-precision/linear-attention inference [32] | `adopt-now as safety harness` | Add precision regression tests before trusting BF16/FP16/quantized outputs for legal/document tasks. Detect loops, malformed JSON, bbox drift, reading-order instability, and triage inconsistency. |

Policy unchanged:

```text
Open research and engineering patterns are allowed.
License-compatible open weights may be evaluated with manifest provenance.
External OCR/layout/grounding systems are not product infrastructure, labelers, verifiers, or hidden dependencies.
```

## 2026-05-28 Layered Document State and Language-Level Update

| Engineering reference | Classification | aKriti interpretation |
|---|---|---|
| layered document-intelligence workbench [33] | `adopt-now as UI/API pattern` | Workbench must expose source/restored/diff layers, block-type filters, language filters, confidence filters, derived artifacts, review states, analyzer votes, and downstream triage/ledger states. |
| compact multilingual document VLM training [34] | `adopt-now as training/eval principle` | A strong 3B/Core model and 8B/Pro model require targeted document data, layout/reading-order supervision, table/chart/formula tasks, multilingual fixtures, and verifiable rewards, not just larger parameter count. |
| formal language-support levels [35] | `adopt-now as manifest/eval policy` | Do not claim a language is supported globally. Track L0-L6 support per language/script and per product surface. |
| edge/on-device runtime and Rust-host packaging [36] | `prototype as runtime target` | Keep training PyTorch/JAX-first, but design runtime packages for replaceable GGUF/MLX/ONNX/LiteRT/Core ML/WebGPU backends. Rust is useful for aKritiParse, local host services, bindings, and safe runtime orchestration. |

Roadmap correction:

```text
Translation, charts, restoration, actions, and exports are first-class aKriti capability families.
Their API contracts, aKritiDoc structures, Workbench layers, filters, provenance rules, and minimal paths must exist early.
Their high-quality model implementations can mature in stages.
```

## 2026-05-28 Feedback and Self-Improvement Update

| Engineering reference | Classification | aKriti interpretation |
|---|---|---|
| self-improving analyzer harness and adapter-update loop [37] | `medium-high after MVP` | Use human-reviewed corrections to improve both Vinti analyzer harnesses and aKriti adapters. All changes must be offline, versioned, evaluated, human-approved, auditable, and rollbackable. |

Correct placement:

```text
aKriti perception/document parsing -> first
Vinti analyzer/voter MVP -> first
feedback/harness/adaptor improvement loop -> after usable correction data exists
```

Allowed update classes:

- harness changes: prompts, retry logic, analyzer thresholds, voting weights, review triggers, rule additions, search/re-read procedure.
- adapter changes: LoRA/QLoRA/OCR correction, Indic glyph ambiguity, layout, legal-document triage, schema extraction.

Blocked in production:

- automatic self-training after a case.
- unapproved prompt rewriting.
- silent threshold changes.
- weight updates without held-out eval.
- ledger records missing model/harness/schema/analyzer versions.

## 2026-05-29 Device Agent and Analyzer Taxonomy Update

| Engineering reference | Classification | aKriti interpretation |
|---|---|---|
| device-optimized active-parameter agent models [38] | `prototype as runtime/orchestrator reference` | Useful for local tool-calling, active-parameter efficiency, tokenizer benchmarking, abstention/loop control, and multi-format package strategy. Not a replacement for aKriti OCR/layout/VLM. |

Terminology lock:

```text
aKriti Analyzer
  reusable document-intelligence analyzer operating on aKritiDoc:
  OCR confidence, language/script ambiguity, grounding, layout, table, chart, entity, restoration drift, precision stability.

Kriti Orchestrator
  generic tool/action coordinator over aKritiDoc:
  ask, inspect, call tool, validate schema, request review, propose action.

Vinti Analyzer Pack
  court-logistics domain analyzers consuming aKritiDoc:
  case type, completeness, readiness, fast-track, ADR/ODR, defects, evidence support, human validation, ledger readiness.
```

The term `Vinti analyzer` is acceptable only for domain-specific court-logistics analyzers. Generic OCR, layout, grounding, translation, restoration, tokenizer, and precision analyzers belong to aKriti.

Adopt from [38]:

- active-parameter efficiency as future `aKriti Pro/Vinti` architecture research.
- local tool-calling orchestrator as a Kriti/Vinti runtime role.
- tokenizer efficiency as a measured Indic/legal benchmark.
- GGUF/MLX/ONNX/vLLM/SGLang-style multi-format packaging as product discipline.
- abstention and loop-control training as safety targets.

Reject for now:

- using a text-only agent as a document VLM replacement.
- training MoE first.
- dumping entire case files into long context instead of using aKritiDoc retrieval and evidence pointers.
- exposing raw chain-of-thought in court/legal workflows.

## 2026-05-30 Deterministic PDF Grid Projection Update

| Engineering reference | Classification | aKriti interpretation |
|---|---|---|
| deterministic grid projection for born-digital PDF text layers [39] | `adopt-now for aKritiParse` | Implement an owned model-free fast path that reconstructs readable aligned text from PDF text coordinates, emits aKritiDoc spans/blocks, and records debug traces. |

Why it matters:

```text
Not every PDF needs VLM/OCR first.
If a text layer exists, deterministic parsing should create cheap, inspectable aKritiDoc evidence.
VLM/OCR should focus on scanned, visual, ambiguous, or semantically hard regions.
```

## 2026-08-02 Deterministic PDF Inspection and Page Routing Update

| Research item | Classification | aKriti use |
|---|---|---|
| model-free PDF operator/font/image inspection and per-page routing | `adopt-now for aKritiParse design` | Independently implement page classification, encoding-quality checks, positioned native-text extraction, one-load document state, and reason-coded escalation to OCR/VLM. External work is research context only, never code, package, runtime, output, label source, teacher, verifier, or benchmark dependency. |

Adopt these system contracts:

- classify and route each page rather than assigning one mode to an entire mixed PDF;
- inspect text-showing operators, image/form objects, font mappings, image coverage, and extraction quality;
- distinguish extractable text from merely present but garbled, invisible, or structurally unusable text;
- parse the document once and share immutable source state across inspection and deterministic extraction;
- retain positioned text, font evidence, source coordinates, route confidence, and fallback reason codes in `aKritiDoc`;
- invoke aKriti Layout/Text/Table/VLM paths only for pages or regions that fail deterministic gates;
- treat Markdown as an export, never as the canonical document representation.

Do not adopt self-reported speed or quality numbers as aKriti claims. Reproduce comparisons on aKriti fixture slices, especially mixed PDFs, Indic CID fonts, court orders, invisible OCR layers, vector tables, multi-column layouts, and malformed encodings.

Implementation rule:

```text
read the public idea and problem statement
    -> write an aKriti-owned design and invariants
    -> implement independently against aKritiDoc
    -> evaluate only with aKriti-owned fixtures and metrics
```

Do not vendor, port, translate, wrap, call, or derive implementation code from the external project. The only external model artifacts that may enter aKriti lineage are explicitly approved, license-compatible open weights when an aKriti experiment proves they are necessary; their provenance must remain visible.

## 2026-08-02 Open-Weight Multimodal Candidate Update

| Candidate class | Classification | aKriti interpretation |
|---|---|---|
| edge multimodal family with roughly 2B-4B effective parameters | `prototype after scaffold/evals exist` | Highest-priority open-weight candidate class for Small/Core comparison. Evaluate document perception, Indic fidelity, structured output, grounding, and sustained local runtime; do not select from provider benchmarks. |
| medium unified image/audio/text decoder with lightweight projections | `prototype for Pro; watch for Core` | Useful encoder-free hypothesis for end-to-end multimodal adaptation, but fine text and document geometry may still require owned document heads or a specialized visual frontend. |
| native-resolution sparse VLM with a small active decoder footprint | `architecture reference; optional candidate` | Study adaptive visual token allocation, native-resolution pages, long-document context, and sparse activation. Admit weights only if license and aKriti-owned fixtures justify an experiment. |
| very-large sparse native multimodal generalist | `reject as local base; watch ideas` | Total weight storage and minimum memory violate local-first targets even when active parameters are smaller. Retain only calibration, abstention, visual crop/zoom tool use, and hybrid-attention hypotheses. |
| older 30B+ OCR/VLM checkpoint | `reject` | A high general OCR score does not justify workstation-scale weights, old architecture, or weak Indic/legal evidence. |
| public VLM leaderboard or survey catalog | `discovery-only` | Use only to find original papers/model cards. Never use aggregate rank as a promotion gate or cite a catalog as proof of capability. |

Architecture ideas admitted for owned experiments:

- per-layer embedding capacity for efficient local models;
- hybrid sliding-window and global attention;
- native-resolution or adaptive-resolution page processing;
- lightweight image/audio projection into a shared decoder;
- sparse expert activation only after dense baselines and profiling;
- calibrated abstention and variable compute as measurable safety/runtime targets;
- tool-assisted crop, zoom, and region inspection through owned aKriti tools.

Open weights remain the only external model artifact that may enter an experiment. No external code, processor implementation, runtime, generated output, teacher signal, labels, or verifier may become part of aKriti.

Build as owned implementation:

- group text fragments into visual lines.
- extract recurring left/right/center anchors.
- classify text items into snap classes.
- separate flowing paragraphs from structured rows.
- project structured text into grid columns.
- preserve source coordinates and debug decisions.
- emit both readable plain text and structured `aKritiDoc` objects.

Do not import external parser behavior as product dependency. Use public algorithm descriptions as design input and implement aKriti-owned code.

## 2026-06-04 Encoder-Free Multimodal and Shruti Update

| Engineering reference | Classification | aKriti interpretation |
|---|---|---|
| encoder-free multimodal projection [40] | `prototype after baseline VLM evals` | Study whether lightweight image/audio projection into a shared document/action backbone can reduce latency and simplify fine-tuning. Do not assume it beats specialized document encoders until aKritiDoc evals prove it. |

Where it fits:

```text
aKriti Core/Pro candidate architecture research.
Kriti Orchestrator local multimodal action loop.
Shruti audio bridge for ASR/TTS/voice commands in LibreOffice.
```

Adopt:

- direct projection as a research architecture for local multimodal models.
- MTP/speculative decoding as runtime-latency technique where package support exists.
- audio input/output requirements for a separate `Shruti` lane.

Reject for now:

- making audio aKriti core scope.
- replacing document OCR/layout heads with encoder-free projection without eval evidence.
- treating model memory as the knowledge base.
- exposing raw thought traces for legal/document workflows.

## 2026-06-19 Sparse-Attention Index Reuse Update

| Engineering reference | Classification | aKriti interpretation |
|---|---|---|
| cross-layer sparse-attention index reuse [41] | `watch / later prototype` | Useful only if a future `aKriti Pro` or `aKriti Vinti` model uses sparse attention where indexer computation is a measured bottleneck. Not relevant to Phase 0/1 parsing, OCR, layout, Workbench, or Vinti MVP. |

Where it fits:

```text
future long-context runtime optimization
server/on-prem Vinti inference
large document/case-bundle synthesis
```

Promotion gate:

- sparse-attention base exists in our candidate set.
- indexer cost is measured and significant.
- reuse strategy improves prefill/decode latency.
- aKritiDoc extraction, citations, bboxes, and Vinti triage do not regress.

Rule:

```text
Structured retrieval beats context dumping.
Index reuse is an optimization after retrieval/indexing architecture is already correct.
```

## 2026-07-08 Open-Weight Multimodal Model-Family Update

| Engineering reference | Classification | aKriti interpretation |
|---|---|---|
| open-weight multimodal model family with encoder-free projection, QAT, speculative/multi-token decoding, and document benchmark coverage [46] | `prototype / benchmark candidate` | Useful for `aKriti Small/Core/Pro` and `Kriti` candidate evaluation. Smaller variants may fit local LibreOffice assist; larger variants may fit Pro, verifier, or teacher roles. Adoption requires aKritiDoc eval wins, runtime manifests, and license/provenance recording. |

Decision unchanged:

```text
Model names do not define aKriti.
aKritiDoc, evals, runtime packages, Workbench review, and safe actions define the product.
```

Promotion gate for [46]:

```text
A candidate from this family can move beyond benchmark/prototype only if it improves OCR/text, layout, table/chart handling, visual grounding, JSON/aKritiDoc validity, local runtime latency/RAM, and unsupported-claim rate on aKriti fixtures.
```

## 2026-10-14 Owned CPU Reading and Task-Aware Local Inference

Plan: [Owned CPU Text Reader and Local Compression](akriti-cpu-text-reader-and-local-compression-plan.md).

| Engineering input | Classification | Independent experiment |
|---|---|---|
| Modular compact CPU line recognition and native-text/visual routing [50] | `reference-only` / `v1.5-experiment` | Owned fast reader, explicit geometry, supported-script slices and calibrated escalation |
| Binary latent factorization and geometry alignment [51]-[52] | `v3-research` | One-projection rank/precision ablation after floating-point and INT8 controls |
| Variable-rank acoustic compression and transition-aware calibration [53]-[54] | `reference-only` | Separate Shruti hypotheses; no claim of novel sub-1-bit ASR |
| Frozen-layer adapter-training streaming [55] | `reference-only` / `do-not-depend` | Training-memory learning, distinct from deployed model compression |
| Shared-depth acoustic processing and early exits [56]-[59] | `reference-only` | Shruti-first experiments; document transfer unproved |
| Modality-aware representation and joint visual budgets [60]-[61] | `v3-research` | Evidence-preserving precision/token experiments, not a generic retention-score shortcut |

No external engine, generated labels, verifier, teacher pipeline or runtime is adopted. Open-weight approval remains separate and license-specific. Named assessments and benchmark/license caveats are kept in the private source note referenced by the plan.

The CPU reader complements the VLM-first platform. Preserve aKritiDoc provenance, exact fields, review and the separate LibreOffice/Vinti/Shruti boundaries. Every card remains planned; package size, process memory, single-page latency and multi-core throughput are different measurements.

## 2026-10-14 Owned Visual Retrieval and Compressed Vector Storage

Plan: [Owned Visual Retrieval Engineering](akriti-visual-retrieval-engineering-plan.md). Canonical boundary: [Retrieval and Grounding](akriti-retrieval-grounding-plan.md), section 5 replaces the old external-store default.

| References | Transfer hypothesis | Required gate |
|---|---|---|
| [62] | Multi-vector page images add discovery recall where OCR/text structure is weak | Compatible encoder contract and our own multilingual page/region fixtures |
| [63] | Compressed scoring plus retained-original rescoring reduces vector traffic while retaining useful ranking | Fixed encoder/query/candidate control, total storage and cold/warm I/O measurements |
| [64]-[65] | Rotation/calibration/asymmetric scoring may improve extreme low-bit retrieval over naive binary codes | Independently implemented codec, versioned metadata and measured recall/latency tradeoff |
| [66] | Separating compact resident vectors and disk originals may help bounded device execution | Peak-memory, mapped-page and rescore I/O accounting; not compressed-byte-only claims |

Sequence: source/page identity and exact lookup -> scoped float MaxSim baseline -> controlled codec/rescore ablations -> coarse candidate indexing only after scale requires it -> LibreOffice/Vinti adapters. EXP-031 through EXP-033 are planned and unrun.

Qdrant and FAISS are research references, not required product/runtime dependencies. Metadata filtering and persistence belong to our owned interfaces. Evidence resolution still opens the original source and uses owned reading/verification; relevance does not settle a character, stamp, table cell or legal claim.

Named-source details and publisher benchmark caveats are in the private working note `LOCAL_DOCS/2026-10-14-visual-page-retrieval-and-vector-compression-research.md`. No model is approved, downloaded or trained, and no engine or performance result is delivered by this entry.

## 2026-10-18 Evidence-Preserving Forms and Budgeted Verification

Plan: [Evidence and Adaptive Compute](akriti-evidence-and-adaptive-compute-research-plan.md). Partner: `../../Shruti/docs/INDIC_EVIDENCE_AND_ADAPTIVE_COMPUTE_RESEARCH_PLAN.md`. References [67]-[73] were checked as research sources, not adopted systems.

Prioritize source-linked extracted text versus typed values versus presentation, script-aware spans, owned deterministic gates and realistic contextual negatives. The existing confidence document now labels historical thresholds as uncalibrated placeholders and removes the assumption that 3/5/7 readings are statistically independent.

| Lane | Hypothesis and gate |
|---|---|
| Critical-unit evidence | Reduce value/unit and forbidden-rewrite errors without hiding glyph or geometry uncertainty |
| Bounded region escalation | Improve the risk/resource frontier versus fixed first/second-pass controls, including model residency and cold costs |
| Selective calibration | Improve accepted-case risk/coverage on sealed and shifted slices; no unconditional correctness certificate |
| Context counterfactuals | Help with relevant names without inserting plausible but unsupported alternatives |

EXP-034 through EXP-037 are planned and unrun. Early-exit/speech-formatting results do not prove OCR improvement. Dataset rights, consent, source revisions, original-versus-derived dependencies and calibration splits are gates, not afterthoughts.

Potential defensibility lies in rights-cleared real failure coverage, predictable local execution, source-review usability and released evaluation discipline. No novelty, durable moat, funding outcome, model approval or performance claim is made. User corrections are local history by default; global training requires separate explicit eligibility and release policy.

## 2026-10-19 Expanded CPU Script Routing and Selective Processing

Plan: [Owned Indic CPU Reader and Selective Processing](akriti-indic-cpu-reader-and-selective-processing-plan.md). Source anchors [74]-[75]; segmentation anchor [67]. Named assessment: `LOCAL_DOCS/2026-10-19-indra-1-1-owned-reader-research.md`.

Update the reference assessment: earlier limited-script, no-handwriting and smaller-bundle notes are historical, not current publisher limitations. Keep experimental handwriting separate from reliable Indic handwriting and total pipeline resources separate from a main recognizer's parameter count.

| Owned idea | Controlled gate |
|---|---|
| Script-aware compact readers | Fixed shared control, mixed/shared/unknown routes, independently labelled script errors and cold/resident resource costs |
| Indic target units and crops | Preserve vowel/conjunct context and source mappings; compare label units without changing capacity/decoder policy simultaneously |
| Selective visual rereads | Reuse EXP-035: one bounded better crop before larger-model escalation; count confidently wrong route misses |
| Contextual ranking | No-LM control and unchanged visual candidates; measure misleading-vocabulary and exact-name/value errors |
| Conditional math | Same available owned specialist with routing ablations; count numeric-table harm and added memory/latency |

EXP-038 through EXP-041 are planned and unrun. No external engine, weights, dictionaries, output-derived labels or verifier is adopted. The first CPU target remains printed Devanagari/Latin with source-linked review, not an unmeasured universal-language or handwriting claim.

## 2026-10-20 Corrected Sequence, Experimental Refinement and Resumable Colab

Plans: [Selective Refinement and Recoverable Routing](akriti-selective-refinement-and-recoverable-routing-research-plan.md), [Resumable Colab Experiments](akriti-colab-resumable-experiment-workflow.md). Source anchors are in the dated reference-index addendum. New cards EXP-042 through EXP-045 are planned and unrun.

| Item | Classification | Decision boundary |
|---|---|---|
| Shared source/geometry fixtures and realistic Indic error categories | Adopt planning/evaluation principles now | No outside OCR labels; no language-coverage claim from a small mechanics set |
| Candidate-coverage diagnostics and bounded source recovery | Prototype after fixed reader controls | Fixed-candidate ranking cannot recover an absent true reading; count all gate/reread costs |
| Recoverable visual-token routing | Later controlled experiment | Some reported architectures/schedules regress; do not promote from theoretical FLOPs or object grounding alone |
| Multimodal diffusion decoding | Later controlled experiment | Distinct from image restoration and from evidence; compare simpler rereads first |
| Mercury 3 | Watch | Vendor GPU text-model signal, not a selected local reader, ASR or hosted dependency |
| Colab | Optional execution environment | Actual capacity, explicit budgets, cloud data eligibility and complete-state recovery required |

Current dependency order is source/aKritiDoc mechanics and fixtures, then document-VLM and complementary CPU-reader baselines alongside each other, then bounded evidence recovery/calibrated review, then isolated runtime or routing comparisons. A full OCR implementation must not become a gate blocking all VLM research.

The Colab workflow distinguishes complete compatible continuation, parent-linked hardware-change continuation and weights-only initialization. It specifies optimizer-boundary snapshots, sampler/RNG progress, immutable generations, completion/hash validation and fallback; none of those mechanisms has been implemented or tested by this entry. Local-device memory/latency and LibreOffice behavior remain separate promotion evidence.

Private documents and corrections are not cloud-training material by default. External model implementations, processors, engines, generated labels, teacher outputs and verifiers remain excluded; any open-weight initialization needs separate approval and lineage. No run, download, model promotion, novelty claim or measured advantage is delivered here.

## 2026-10-22 Typed Decision Reliability and Modality Boundaries

Plan: [Evidence and Adaptive Compute](akriti-evidence-and-adaptive-compute-research-plan.md). Draft: [Shared Local Decision v0](akriti-shared-decision-contract-v0.md). References [76]-[89]. Detailed application notes live in Shruti's `docs/LOCAL_DICTATION_AND_TYPED_DECISION_RESEARCH.md`, not an aKriti dependency list.

| Input | Disposition | Owned interpretation |
|---|---|---|
| Prefill-only typed heads [76] | Reference; general model experimental | Contract/evaluation first, not external engine or universal weights |
| Missing-evidence audit [77] | Adopt evaluation principle | Confidence does not establish evidence sufficiency |
| Choice-set dependence [78] | Adopt evaluation principle | Version ordered options; test distractors/omissions |
| Ordinal-scale compression [79] | Watch/prototype | Validate scale use; an expected rating is not calibrated risk |
| Acoustic confidence/context/repair [80]-[84] | Shruti prototypes; visual hypotheses only | Source evidence and full cost, not fluency or imported teacher labels |
| Indic conditions [85] | Benchmark design | Named language/recording slices, independent grouped labels |
| Low-bit and commit/turn work [86]-[88] | Later experiments | No packed-device, streaming or OCR gain implied |
| Noise-aware precision training [89] | Prior-art reference | QAT/noisy-student combinations are not automatically novel |

EXP-046 through EXP-049 are planned and unrun. Keep acoustic and visual heads independent and calibration scope explicit. A general Jev-like question model cannot block document foundations or ASR.

Correct the comparison taxonomy: preview, native causal recognition, streaming upload and final-file recognition differ; artifact/resident/process memory differ; public code, code licence and weight licence differ. Static source review does not prove privacy, platform parity or roadmap delivery.

No app, model implementation, normalizer, aligner, SDK, labels, teacher output or verifier is adopted. Any open weights need separate approval/lineage. Public architecture choices establish neither novelty nor a durable moat.
