# aKriti Decision Log

**Status:** Locked decisions as of 2026-05-20

## 1. Locked decisions

### D001: aKriti is VLM-first, not OCR-first

Decision:

```text
aKriti is a document VLM/model-family platform with OCR/text reading as one owned capability.
```

Reason:
- OCR alone cannot handle layout, charts, tables, visual grounding, transformations, and reasoning.
- VLM alone still needs validation and structured document contracts.

### D002: aKriti is not a wrapper

Decision:

```text
No external OCR/VLM system is a final product dependency.
```

Open weights may be used as initialization or raw model material with honest lineage. Public projects, OCR/VLM systems, papers, repos, model cards, and demos may be studied for engineering ideas only. Shipped modules, labels, verifiers, runtimes, APIs, and product behavior must be aKriti-owned or implemented in-house.

### D003: aKritiDoc is the canonical primitive

Decision:

```text
Every parser, model, verifier, UI, and exporter writes through aKritiDoc.
```

Reason:
- prevents multiple truths
- enables provenance
- enables deterministic validation
- enables LibreOffice-native edits
- enables Vinti/court evidence flows

### D004: open-weight base-family candidate/open-weight base-family candidate are candidate bases, not identity

Decision:

```text
Evaluate open-weight base-family candidate now and open-weight base-family candidate when stable/open.
Do not hardcode aKriti to open-weight base-family candidate.
```

Reason:
- open-weight base-family candidate currently looks strong, but base models change quickly.
- The system moat is schema, data, evals, runtime, UI, and integration.

### D005: Diffusion is restoration-only for now

Decision:

```text
Use diffusion/rectification for image quality restoration only.
Do not use diffusion as the core parser/reasoning model.
```

Reason:
- restoration can improve readability
- diffusion can hallucinate text-like strokes
- legal/document evidence needs original-preserving provenance

### D006: Exact search is first-class

Decision:

```text
Use exact/layout/entity search before vector search for document QA.
```

Reason:
- dates, amounts, names, sections, citations, and table cells need exactness.
- vector-only RAG can miss critical details.

### D007: Verification is mandatory

Decision:

```text
Every model output must pass schema, provenance, confidence, deterministic checks, or user approval.
```

Reason:
- LLM/VLM outputs are stochastic.
- High-stakes docs require auditability.

### D008: LibreOffice is a first-class native target

Decision:

```text
LibreOffice integration should be native C++/UNO/sidebar/canvas oriented, not merely an extension wrapper.
```

Reason:
- user has LibreOffice core experience
- document-native edits matter
- upstream-compatible architecture should respect LibreOffice internals

### D009: Product is API-first

Decision:

```text
aKriti must expose stable local APIs so any UI/software can plug into it.
```

Targets:
- Workbench
- LibreOffice
- FilterTube
- Vinti
- desktop/mobile/browser apps

### D010: Tokenizer and Hinglish policy

Decision:

```text
Do not swap tokenizers inside pretrained external VLM/LLM bases in v1.
For owned student models, evaluate a grapheme-aware Unigram tokenizer with byte fallback.
Use Hinglish as augmentation/alignment data, not as a replacement for native-script fidelity.
```

Reason:
- pretrained model tokenizers are part of the learned distribution and changing them early creates avoidable instability.
- Indic document intelligence needs native-script fidelity, not only romanized code-mixed handling.
- a custom tokenizer is justified only when it improves measured OCR/text, translation, compression, or runtime outcomes.

### D011: Benchmark and claim lock

Decision:

```text
No component, model, parser, runtime, or release claim should be promoted as "better" without measurements.
```

Required evidence:
- CER/WER and Indic-script CER
- structure/layout accuracy
- grounding and provenance coverage
- reading-order accuracy
- table and chart reconstruction quality
- confidence calibration
- restoration deltas against original scans
- Indic and code-mixed stress tests
- latency, RAM/VRAM, package size, and CPU/GPU behavior for local targets

### D012: Visual blocks are first-class document artifacts

Decision:

```text
Images, figures, plots, charts, signatures, stamps, diagrams, and embedded visual regions are source artifacts in aKritiDoc.
Generated captions/descriptions are derived interpretations and must stay separate from source text.
```

Required metadata:
- bbox and page coordinates
- provenance back to original file/page/region
- artifact type
- extraction confidence
- optional generated caption/description
- optional chart/table reconstruction linked to the source visual region

Reason:
- LibreOffice, PDFs, court documents, and reports often carry meaning in non-text regions.
- treating visuals as first-class blocks enables chart QA, figure extraction, evidence review, and safe native edits.

### D013: Ownership wording must be precise

Decision:

```text
If base weights start from open weights, say so.
Do not claim parser weights are fully ours until the checkpoint/module is actually owned.
It is correct to say the aKriti system, schema, API, runtime packaging, evals, and integration layer are ours.
```

Reason:
- this protects technical honesty.
- it separates product ownership from model-weight provenance.
- it keeps the long-term owned-model path clear without overstating v1.

## 2. Rejected or delayed paths

| Path | Status | Reason |
|---|---|---|
| wrapper around named external OCR/layout systems | rejected for final product | violates ownership |
| OCR-only product | rejected | insufficient for document intelligence |
| vector-RAG-only document chat | rejected | misses exact legal/document facts |
| diffusion as core LLM/VLM | delayed/research-only | not stable/open/product-suitable enough |
| training full VLM from scratch immediately | rejected now | requires data/evals first |
| internal MoE/5 agents inside model in v1 | rejected now | external evidence/harness first |
| court product as core repo scope | delayed/downstream | core must be general multilingual document intelligence |
| zero-native as main LibreOffice UI path | rejected now | poor fit for native LibreOffice |

## 3. Research-only lanes

These are interesting but must not block v1:
- hierarchical attention reference
- token-superposition-style pretraining reference
- neuron-preserving optimizer optimizer
- four-bit pretraining reference
- tile-wise sparse FFN
- world-model reference/world-model reference world models
- dLLM/diffusion text
- Natural Language Autoencoders
- Goodfire-style neural geometry probes

## 4. Near-term experiment lanes

Allowed after schema/API/eval skeleton exists:
- open-weight base-family candidate/open-weight base-family candidate base evaluation
- PEFT/LoRA/QLoRA/DoRA/LoRA+/adaptive low-rank training reference adapters
- extreme quantization reference/llama.cpp KV cache compression
- MLX/GGUF/ONNX runtime export
- WebGPU FilterTube tiny model prototype
- external document-layout reference-style visual layout UI
- CURIO-style dewarp/rectification

## 5. Final module naming decision

Use aKriti-owned names:

```text
aKriti Layout Reader
aKriti Text Reader
aKriti Table Reader
aKriti Chart Reader
aKriti Image Reader
aKriti Restoration Module
aKriti Translation Module
Kriti Reasoning/Action Module
aKriti Runtime
```

Do not describe final architecture as using external specialist engines.

## 6. Model tier decision

```text
aKriti Tiny  -> routing/embeddings/thumbnail/page triage
aKriti Small -> local OCR assist and simple document understanding
aKriti Core  -> around 3B main local document VLM/reasoning
aKriti Pro   -> 8B+ teacher/verifier/cloud/workstation
Kriti        -> reasoning/action/document-command module
```

## 7. Training ownership decision

Stages:

```text
1. harness and schema
2. open-weight adapters
3. distilled owned checkpoints
4. independent owned modules
5. local runtime packaging
```

Only after stage 3 should public language say "our model family" without caveat.

## 8. Safety and evidence decision

For legal/court and user documents:

```text
original remains evidence
restored output is derived artifact
generated summary is interpretation
native edit requires approval
all high-impact claims need provenance
```

## 9. Next model watch decision

Only one near-term external model event is allowed to affect base selection:

```text
open-weight base-family candidate open-weight release and model card.
```

All other new papers must enter the research ledger, not derail the build path.

### D014: External systems are inspiration-only; open weights are the only importable model artifact

**Decision:** aKriti will not depend on external OCR, VLM, PDF, translation, layout, video, restoration, or reasoning systems as product components, teacher systems, verifier systems, or label generators. External projects and papers may be studied, but the implementation must be aKriti-owned. The only external model artifact allowed into aKriti lineage is open weights, with license/provenance recorded in the model manifest.

**Rationale:** The project goal is an owned local-first document intelligence model family that can be integrated natively into LibreOffice, FilterTube, and later Vinti. Wrapping existing systems would not produce the ownership, portability, privacy, or low-compute control that the project needs.

## Research References

This doc is connected to the numbered research bibliography in `docs/akriti-research-reference-index.md`. Those references are engineering anchors for aKriti-owned implementation; they are not product dependencies. Only open weights may enter model lineage, and only with manifest provenance.
