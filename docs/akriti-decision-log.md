# aKriti Decision Log

**Status:** Locked decisions as of 2026-05-19

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

Open weights may be used as initialization/teacher material. Public projects may be studied for ideas. Shipped modules must be aKriti-owned.

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

### D004: Qwen3.6/Qwen3.7 are candidate bases, not identity

Decision:

```text
Evaluate Qwen3.6 now and Qwen3.7 when stable/open.
Do not hardcode aKriti to Qwen.
```

Reason:
- Qwen currently looks strong, but base models change quickly.
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

## 2. Rejected or delayed paths

| Path | Status | Reason |
|---|---|---|
| wrapper around GLM-OCR/DeepSeek/Hunyuan/Paddle | rejected for final product | violates ownership |
| OCR-only product | rejected | insufficient for document intelligence |
| vector-RAG-only document chat | rejected | misses exact legal/document facts |
| diffusion as core LLM/VLM | delayed/research-only | not stable/open/product-suitable enough |
| training full VLM from scratch immediately | rejected now | requires data/evals first |
| internal MoE/5 agents inside model in v1 | rejected now | external evidence/harness first |
| court product as core repo scope | delayed/downstream | core must be general multilingual document intelligence |
| zero-native as main LibreOffice UI path | rejected now | poor fit for native LibreOffice |

## 3. Research-only lanes

These are interesting but must not block v1:
- Lighthouse Attention
- Token Superposition Training
- Aurora optimizer
- NVFP4
- TwELL sparse FFN
- JEPA/LeWM world models
- dLLM/diffusion text
- Natural Language Autoencoders
- Goodfire-style neural geometry probes

## 4. Near-term experiment lanes

Allowed after schema/API/eval skeleton exists:
- Qwen3.6/Qwen3.7 base evaluation
- PEFT/LoRA/QLoRA/DoRA/LoRA+/AdaPaD adapters
- TurboQuant/llama.cpp KV cache compression
- MLX/GGUF/ONNX runtime export
- WebGPU FilterTube tiny model prototype
- HURIDOCS-style visual layout UI
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
Qwen3.7 open-weight release and model card.
```

All other new papers must enter the research ledger, not derail the build path.
