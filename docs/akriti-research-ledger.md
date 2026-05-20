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
