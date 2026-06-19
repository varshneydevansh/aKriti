---
name: akriti-vlm-project
description: Use for any aKriti, Kriti, Vinti, FilterTube, LibreOffice document-intelligence, OCR/VLM, multilingual document AI, local runtime, model-family, API, or architecture work. Grounds decisions in the aKriti canonical docs and LOCAL_DOCS chat/research memory.
---

# aKriti VLM Project

## First files to read

Use the smallest set needed for the task:

- `/Users/devanshvarshney/aKriti/docs/akriti-decision-log.md` for locked boundaries and watch items.
- `/Users/devanshvarshney/aKriti/docs/akriti-master-architecture.md` for the model/module/runtime/product architecture.
- `/Users/devanshvarshney/aKriti/docs/akriti-api-capability-map.md` for user-facing capabilities and endpoints.
- `/Users/devanshvarshney/aKriti/docs/akriti-research-ledger.md` when evaluating papers, models, OCR systems, VLM systems, optimizers, or runtimes.
- `/Users/devanshvarshney/aKriti/docs/akriti-implementation-plan.md` when planning build phases.
- `/Users/devanshvarshney/aKriti/LOCAL_DOCS/codex_chat1.md` and `/Users/devanshvarshney/aKriti/LOCAL_DOCS/codex_chat2(main).md` only when canonical docs are insufficient or the user asks for historical context.

## Non-negotiables

- aKriti is not a wrapper around external OCR/VLM products.
- Open weights may be used as starting material, teachers, baselines, or distillation sources.
- Final product direction is owned modules, owned APIs, owned runtime packaging, and owned evals.
- OCR/document specialists such as GLM-OCR, DeepSeek-OCR, HunyuanOCR, PaddleOCR, and HURIDOCS are references/baselines, not dependencies to expose as the product.
- `aKritiDoc` is the canonical intermediate representation for pages, blocks, text spans, tables, charts, images, provenance, edits, and verification.
- Exact text search comes before vector search for legal/document workflows. Vectors are a semantic layer, not a replacement for exact citations.
- Verification is mandatory for high-stakes document changes: every answer should trace to page/region/source evidence when possible.
- Diffusion/restoration is a non-destructive enhancement path for degraded scans and character repair, not the core parser or reasoning engine.

## Architecture vocabulary

Use these module names consistently:

- `aKriti Layout Reader`
- `aKriti Text Reader`
- `aKriti Table Reader`
- `aKriti Chart Reader`
- `aKriti Image Reader`
- `aKriti Restoration Module`
- `aKriti Translation Module`
- `Kriti Reasoning/Action Module`
- `aKriti Runtime`

Use these product surfaces consistently:

- `aKriti Workbench`
- `LibreOffice native sidebar/canvas integration`
- `FilterTube local semantic filtering`
- `Vinti court-document product`

## Model-family assumptions

- `aKriti Tiny`: routing, embeddings, thumbnails, low-compute page triage.
- `aKriti Small`: local OCR assist, image/page understanding, CPU/RAM-friendly tasks.
- `aKriti Core`: around 3B, primary local document VLM/reasoning model.
- `aKriti Pro`: 8B+ teacher, verifier, workstation/cloud model.
- `Kriti`: reasoning/action layer on top of the model family.

## Research handling

When the user brings new papers, repos, tweets, model releases, or optimizer/runtime ideas:

1. Classify as `adopt now`, `prototype`, `watch`, or `reject`.
2. Explain the relevance to aKriti Core, aKriti Tiny/Small, LibreOffice, FilterTube, and Vinti separately when useful.
3. Prefer techniques that preserve local/offline deployment and clear verification.
4. Update or propose updates to the research ledger and decision log if the information changes the roadmap.

## Implementation posture

- Be concrete: convert ideas into phases, specs, evals, schemas, API contracts, and runtime targets.
- Default runtime targets: GGUF/llama.cpp, MLX, ONNX Runtime, LiteRT, Core ML, WebGPU/WASM, CUDA/TensorRT.
- Default engineering language split: C++/UNO for native LibreOffice integration, Rust or C++ for safety-critical local services, Python/JAX/PyTorch for research training, TypeScript/WebGPU for browser surfaces, Swift/Kotlin only for native mobile app shells when needed.
- Do not run expensive training, model downloads, or benchmark jobs unless the user explicitly asks.
