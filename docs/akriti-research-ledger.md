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
