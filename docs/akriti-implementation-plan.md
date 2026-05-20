# Plan: aKriti VLM Model Family and Platform

**Generated:** 2026-05-19  
**Estimated Complexity:** High  
**Planning mode:** `plan-harder` + `planner` synthesis  

## Overview

Build aKriti as an owned local-first multilingual document intelligence platform with a model family, canonical `aKritiDoc` structure, APIs, visual workbench, LibreOffice-native integration path, FilterTube model path, and Vinti/court downstream path.

The plan avoids permanent dependency on external OCR/VLM systems. Open weights may be used as initialization/teacher material. Public OCR/VLM/layout projects are references, baselines, or temporary R&D tools only.

## Prerequisites

- Python research stack
- TypeScript frontend stack
- future C++ LibreOffice bridge
- access to local PDFs/images/docs for seed tests
- clear license tracking for datasets, weights, and derived checkpoints
- GPU/cloud access later for serious model training

## Sprint 0: Source-of-truth documentation

**Goal:** Lock architecture and prevent research drift.

**Demo/Validation:**
- Docs exist in `docs/`.
- A new contributor can understand product scope, model ownership, phases, APIs, and research lanes from docs alone.

### Task 0.1: Create master architecture

- **Location:** `docs/akriti-master-architecture.md`
- **Description:** Define ownership, product surfaces, model tiers, module architecture, runtime, LibreOffice/FilterTube/Vinti paths.
- **Complexity:** 3
- **Dependencies:** none
- **Acceptance Criteria:**
  - Includes ASCII and Mermaid diagrams.
  - Explicitly states no external OCR/VLM product dependencies.
  - Defines aKriti model family and owned modules.
- **Validation:** Manual review.

### Task 0.2: Create API/capability map

- **Location:** `docs/akriti-api-capability-map.md`
- **Description:** Define capabilities and local API endpoints.
- **Complexity:** 3
- **Dependencies:** Task 0.1
- **Acceptance Criteria:**
  - Covers translation, extraction, conversion, OCR/text, images, charts, tables, restore, verify, apply-edit.
  - Includes LibreOffice and FilterTube flows.
- **Validation:** Manual endpoint checklist.

### Task 0.3: Create research ledger

- **Location:** `docs/akriti-research-ledger.md`
- **Description:** Classify research items from `LOCAL_DOCS` and provider-native code agent harness chats.
- **Complexity:** 3
- **Dependencies:** none
- **Acceptance Criteria:**
  - Includes open-weight base-family candidate, external OCR specialist, external OCR specialist, external OCR specialist, external OCR/layout reference, extreme quantization reference, LiteRT, MLX, WebGPU, adaptive low-rank training reference, token-bag pretraining, training-only hierarchical attention, neuron-preserving optimizer, world-model reference, diffusion/restoration, external document-layout reference, Orion, DFT.
  - Labels each item by roadmap role.
- **Validation:** Manual term coverage check.

### Task 0.4: Create decision log

- **Location:** `docs/akriti-decision-log.md`
- **Description:** Record locked, rejected, delayed, and watch decisions.
- **Complexity:** 2
- **Dependencies:** Task 0.1
- **Acceptance Criteria:**
  - Locks VLM-first, not wrapper, aKritiDoc-first, exact-search-first, verification mandatory.
  - Defines open-weight base-family candidate as watch-only.
- **Validation:** Manual review.

## Sprint 1: Repository skeleton and aKritiDoc schema

**Goal:** Implement the canonical representation and package skeleton.

**Demo/Validation:**
- Can instantiate a minimal `aKritiDoc`.
- Can serialize/deserialize it.
- Can validate required fields.

### Task 1.1: Create core package layout

- **Location:** `core/akriti_doc/`, `core/ingestion/`, `core/modules/`, `core/runtime/`
- **Description:** Add empty packages and README stubs.
- **Complexity:** 2
- **Dependencies:** Sprint 0
- **Acceptance Criteria:**
  - Package paths exist.
  - Responsibilities are documented.
- **Validation:** import smoke test later.

### Task 1.2: Define `aKritiDoc` schema v0

- **Location:** `core/akriti_doc/schema.py`
- **Description:** Define document, page, block, span, table, chart, image, provenance, transform, quality models.
- **Complexity:** 5
- **Dependencies:** Task 1.1
- **Acceptance Criteria:**
  - Supports normalized coordinates.
  - Supports source artifact hashes.
  - Supports confidence and provenance fields.
  - Supports versioning.
- **Validation:** schema unit tests.

### Task 1.3: Add schema validators

- **Location:** `core/akriti_doc/validators.py`
- **Description:** Validate bbox ranges, page references, block ordering, table references, provenance references.
- **Complexity:** 5
- **Dependencies:** Task 1.2
- **Acceptance Criteria:**
  - Rejects invalid bboxes.
  - Rejects missing page/block refs.
  - Emits warnings for low confidence/missing provenance.
- **Validation:** unit tests.

### Task 1.4: Add basic exporters

- **Location:** `core/akriti_doc/exporters.py`
- **Description:** Convert minimal aKritiDoc to JSON, Markdown, and HTML views.
- **Complexity:** 4
- **Dependencies:** Task 1.2
- **Acceptance Criteria:**
  - JSON is lossless for schema fields.
  - Markdown/HTML are views, not canonical truth.
- **Validation:** snapshot tests.

## Sprint 2: Ingestion and deterministic fast path

**Goal:** Parse born-digital PDFs and basic documents without rasterizing everything.

**Demo/Validation:**
- Submit simple PDF and receive aKritiDoc with text blocks.

### Task 2.1: Add ingestion interface

- **Location:** `core/ingestion/base.py`
- **Description:** Define input artifact, page image/text object, and ingestion result interfaces.
- **Complexity:** 3
- **Dependencies:** Sprint 1
- **Acceptance Criteria:**
  - Supports file path and metadata.
  - Supports born-digital/scanned classification placeholder.
- **Validation:** unit tests.

### Task 2.2: Add born-digital PDF text extraction

- **Location:** `core/ingestion/pdf_text.py`
- **Description:** Extract text and basic page structure from PDF into aKritiDoc.
- **Complexity:** 5
- **Dependencies:** Task 2.1
- **Acceptance Criteria:**
  - Does not rasterize text-first PDFs.
  - Preserves page numbers and source artifact provenance.
- **Validation:** sample PDF test.

### Task 2.3: Add page rendering hook

- **Location:** `core/ingestion/render.py`
- **Description:** Render pages only when needed for scanned/visual/layout flow.
- **Complexity:** 4
- **Dependencies:** Task 2.1
- **Acceptance Criteria:**
  - Produces image artifact metadata.
  - Records render DPI and transform.
- **Validation:** rendering smoke test.

## Sprint 3: Visual workbench prototype

**Goal:** Make aKritiDoc visible and inspectable.

**Demo/Validation:**
- Open workbench.
- View PDF/page image, block overlays, JSON/Markdown/HTML.

### Task 3.1: Create frontend app shell

- **Location:** `workbench/`
- **Description:** Add TypeScript/React frontend skeleton.
- **Complexity:** 4
- **Dependencies:** Sprint 1
- **Acceptance Criteria:**
  - Loads local demo aKritiDoc.
  - Shows document and side panel.
- **Validation:** browser manual test.

### Task 3.2: Add page viewer and overlays

- **Location:** `workbench/src/components/DocumentViewer.*`
- **Description:** Render page images and block boxes.
- **Complexity:** 6
- **Dependencies:** Task 3.1
- **Acceptance Criteria:**
  - Click block to inspect metadata.
  - Low-confidence blocks visibly marked.
- **Validation:** manual fixture review.

### Task 3.3: Add synchronized views

- **Location:** `workbench/src/components/ExportViews.*`
- **Description:** JSON, Markdown, HTML views over same aKritiDoc.
- **Complexity:** 5
- **Dependencies:** Task 3.1
- **Acceptance Criteria:**
  - Views remain synchronized with selected block.
- **Validation:** manual demo.

## Sprint 4: API server

**Goal:** Local API that any UI/software can call.

**Demo/Validation:**
- `POST /v1/parse` returns job id.
- `GET /v1/parse/{job_id}` returns aKritiDoc.

### Task 4.1: Add API skeleton

- **Location:** `api/`
- **Description:** Local FastAPI or equivalent service for parse/search/ask endpoints.
- **Complexity:** 4
- **Dependencies:** Sprint 1
- **Acceptance Criteria:**
  - Health endpoint.
  - Models/capabilities endpoint.
- **Validation:** API smoke tests.

### Task 4.2: Add parse endpoint

- **Location:** `api/routes/parse.py`
- **Description:** Wire ingestion to aKritiDoc output.
- **Complexity:** 5
- **Dependencies:** Sprint 2, Task 4.1
- **Acceptance Criteria:**
  - Supports file upload/path.
  - Returns status and output artifacts.
- **Validation:** integration test.

### Task 4.3: Add search endpoint

- **Location:** `api/routes/search.py`
- **Description:** Exact/layout search over aKritiDoc.
- **Complexity:** 5
- **Dependencies:** Sprint 1, Task 4.1
- **Acceptance Criteria:**
  - Exact text search.
  - Page/block filtering.
  - Provenance result refs.
- **Validation:** unit tests.

## Sprint 5: Owned module interfaces

**Goal:** Define internal aKriti modules before plugging models.

**Demo/Validation:**
- Mock modules produce typed outputs that normalize into aKritiDoc.

### Task 5.1: Define module interfaces

- **Location:** `core/modules/base.py`
- **Description:** Interfaces for Layout Reader, Text Reader, Table Reader, Chart Reader, Image Reader, Restoration, Translation, Kriti Action.
- **Complexity:** 4
- **Dependencies:** Sprint 1
- **Acceptance Criteria:**
  - No external engine-specific abstractions leak into core.
  - Inputs/outputs are typed.
- **Validation:** type/unit tests.

### Task 5.2: Add mock visual modules

- **Location:** `core/modules/mock/`
- **Description:** Deterministic mock modules for UI/API development.
- **Complexity:** 3
- **Dependencies:** Task 5.1
- **Acceptance Criteria:**
  - Can generate sample blocks/tables/charts/images.
- **Validation:** fixture tests.

### Task 5.3: Add router skeleton

- **Location:** `core/routing/router.py`
- **Description:** Decide capability route from file type, confidence, user target, hardware capability.
- **Complexity:** 5
- **Dependencies:** Task 5.1
- **Acceptance Criteria:**
  - Supports `fast`, `balanced`, `accurate`.
  - Supports target-specific routes.
- **Validation:** unit tests.

## Sprint 6: Evaluation harness

**Goal:** Make correctness measurable before training.

**Demo/Validation:**
- Run evaluation on fixture set and produce report.

### Task 6.1: Add metric skeleton

- **Location:** `evaluation/metrics/`
- **Description:** Add CER/WER, bbox IoU, reading order, table structure, chart data, schema validity, provenance metrics.
- **Complexity:** 6
- **Dependencies:** Sprint 1
- **Acceptance Criteria:**
  - Metrics accept prediction and gold aKritiDoc.
- **Validation:** metric unit tests.

### Task 6.2: Add fixture dataset format

- **Location:** `data/fixtures/`
- **Description:** Define small local gold-set structure.
- **Complexity:** 4
- **Dependencies:** Sprint 1
- **Acceptance Criteria:**
  - Includes simple born-digital, scanned, table, chart, mixed-script examples.
- **Validation:** fixture loader test.

### Task 6.3: Add evaluation CLI

- **Location:** `evaluation/run_eval.py`
- **Description:** Run parser on fixture set and produce Markdown/JSON report.
- **Complexity:** 5
- **Dependencies:** Tasks 6.1, 6.2
- **Acceptance Criteria:**
  - Outputs per-document and aggregate metrics.
- **Validation:** local eval run.

## Sprint 7: Restoration and character repair lane

**Goal:** Add non-destructive restoration infrastructure.

**Demo/Validation:**
- Create restored artifact from crop/page and compare original/restored reads.

### Task 7.1: Define restoration artifact schema

- **Location:** `core/akriti_doc/restoration.py`
- **Description:** Store original, transform, restored artifact, parameters, warnings.
- **Complexity:** 4
- **Dependencies:** Sprint 1
- **Acceptance Criteria:**
  - Original immutable.
  - Restored marked derived.
- **Validation:** schema tests.

### Task 7.2: Add deterministic cleanup baseline

- **Location:** `core/modules/restoration/deterministic.py`
- **Description:** Add basic dewarp/denoise/contrast hooks before any diffusion.
- **Complexity:** 5
- **Dependencies:** Task 7.1
- **Acceptance Criteria:**
  - Produces derived artifact with provenance.
- **Validation:** image fixture comparison.

### Task 7.3: Add restoration verification policy

- **Location:** `core/modules/restoration/verify.py`
- **Description:** Compare original/restored reads and flag disagreement.
- **Complexity:** 5
- **Dependencies:** Task 7.1
- **Acceptance Criteria:**
  - Disagreement lowers confidence.
  - Does not replace original evidence.
- **Validation:** unit tests.

## Sprint 8: Model adapter experiments

**Goal:** Start using open weights as raw material while keeping owned outputs.

**Demo/Validation:**
- Train or load a tiny adapter for a controlled toy task.

### Task 8.1: Define training data format

- **Location:** `training/data_formats/akriti_supervised.md`, `training/data_formats/*.py`
- **Description:** Define supervised examples from input artifact to aKritiDoc/action target.
- **Complexity:** 5
- **Dependencies:** Sprint 1
- **Acceptance Criteria:**
  - Supports OCR/layout/table/chart/translation/action examples.
- **Validation:** sample loader test.

### Task 8.2: Add PEFT experiment harness

- **Location:** `training/peft/`
- **Description:** Compare LoRA/QLoRA/DoRA/LoRA+/adaptive low-rank training reference where available.
- **Complexity:** 7
- **Dependencies:** Task 8.1
- **Acceptance Criteria:**
  - Can run small adapter experiment.
  - Logs config, metrics, artifacts.
- **Validation:** toy training run.

### Task 8.3: Add model candidate registry

- **Location:** `training/model_registry.yaml`
- **Description:** Track open-weight base-family candidate, open-weight base-family candidate watch, open-weight edge-model reference, and other open-weight candidates.
- **Complexity:** 3
- **Dependencies:** none
- **Acceptance Criteria:**
  - Includes license, role, runtime/export support, status.
- **Validation:** manual review.

## Sprint 9: Runtime abstraction

**Goal:** Decouple aKriti from one inference stack.

**Demo/Validation:**
- Same mock prompt/task can route to a mock backend and one real local backend later.

### Task 9.1: Define runtime interface

- **Location:** `core/runtime/base.py`
- **Description:** Interface for generation, vision input, embeddings, model metadata, capabilities.
- **Complexity:** 5
- **Dependencies:** Sprint 5
- **Acceptance Criteria:**
  - Supports model tier/capability discovery.
  - Supports streaming and non-streaming result placeholders.
- **Validation:** unit tests.

### Task 9.2: Add backend registry

- **Location:** `core/runtime/registry.py`
- **Description:** Register backends for mock, llama.cpp/GGUF, MLX, ONNX, LiteRT, WebGPU, Core ML.
- **Complexity:** 5
- **Dependencies:** Task 9.1
- **Acceptance Criteria:**
  - No backend-specific dependency required until selected.
- **Validation:** registry tests.

### Task 9.3: Add hardware/capability detector

- **Location:** `core/runtime/capabilities.py`
- **Description:** Detect CPU/RAM/GPU/runtime availability for routing.
- **Complexity:** 5
- **Dependencies:** Task 9.1
- **Acceptance Criteria:**
  - Returns safe capability object.
- **Validation:** local smoke test.

## Sprint 10: LibreOffice native prototype plan

**Goal:** Prepare native integration without coupling too early.

**Demo/Validation:**
- Documented C++/UNO bridge contract and mock local API action.

### Task 10.1: Define LibreOffice action schema

- **Location:** `integrations/libreoffice/action_schema.md`
- **Description:** Define selection context and edit deltas for Writer/Calc/Impress.
- **Complexity:** 5
- **Dependencies:** Sprint 4
- **Acceptance Criteria:**
  - Covers text, table, chart, image, page selection.
  - Supports preview/apply/rollback.
- **Validation:** review against LibreOffice use cases.

### Task 10.2: Define sidebar UX spec

- **Location:** `integrations/libreoffice/sidebar_spec.md`
- **Description:** Selection-aware chat/canvas, provenance, preview, confidence UI.
- **Complexity:** 4
- **Dependencies:** Task 10.1
- **Acceptance Criteria:**
  - Includes user flows for translate, extract table, read chart, rewrite.
- **Validation:** manual UX review.

## Sprint 11: FilterTube local semantic path

**Goal:** Use aKriti Tiny ideas for browser/mobile semantic filtering.

**Demo/Validation:**
- A toy classifier pipeline classifies title/thumbnail fixtures.

### Task 11.1: Define FilterTube capability contract

- **Location:** `integrations/filtertube/semantic_filter_contract.md`
- **Description:** Define input metadata, model tiers, decisions, cache, privacy rules.
- **Complexity:** 4
- **Dependencies:** Sprint 9
- **Acceptance Criteria:**
  - Rules-first cascade documented.
  - VLM fallback only for ambiguity.
- **Validation:** review against browser constraints.

### Task 11.2: Add thumbnail/keyframe dataset format

- **Location:** `data/filtertube/`
- **Description:** Define local dataset format for title/channel/thumbnail labels.
- **Complexity:** 4
- **Dependencies:** Task 11.1
- **Acceptance Criteria:**
  - Supports semantic categories and user rules.
- **Validation:** loader tests later.

## Sprint 12: Vinti/court downstream specification

**Goal:** Keep court product separate but prepared.

**Demo/Validation:**
- Vinti spec references aKriti APIs without polluting core.

### Task 12.1: Define court-document capability map

- **Location:** `integrations/vinti/court_document_capabilities.md`
- **Description:** Chronology, parties, dates, exhibits, procedural stage, provenance.
- **Complexity:** 5
- **Dependencies:** Sprint 4
- **Acceptance Criteria:**
  - States AI does not judge.
  - Uses aKritiDoc/evidence graph.
- **Validation:** legal-safety review later.

## Sprint 13: Owned model family training

**Goal:** Move from adapters to owned checkpoints.

**Demo/Validation:**
- Produce first internal aKriti Tiny/Small checkpoint from controlled data.

### Task 13.1: Build synthetic document generator

- **Location:** `data/synthetic/`
- **Description:** Generate multilingual pages, tables, charts, stamps, degradation, mixed scripts.
- **Complexity:** 8
- **Dependencies:** Sprint 6
- **Acceptance Criteria:**
  - Outputs image/PDF + gold aKritiDoc.
  - Supports Indic scripts and degradation profiles.
- **Validation:** visual/sample review and gold schema validation.

### Task 13.2: Add distillation pipeline

- **Location:** `training/distillation/`
- **Description:** Convert aKriti-owned internal mentor outputs, manifest-recorded open-weight-derived outputs, and human corrections into supervised targets.
- **Complexity:** 8
- **Dependencies:** Tasks 8.1, 13.1
- **Acceptance Criteria:**
  - Records source/teacher provenance.
  - Filters low-confidence pseudo-labels.
- **Validation:** dataset quality report.

### Task 13.3: Train aKriti Tiny

- **Location:** `training/runs/akriti_tiny/`
- **Description:** Train small router/embedding/triage model.
- **Complexity:** 8
- **Dependencies:** Tasks 13.1, 13.2
- **Acceptance Criteria:**
  - Beats simple heuristic baseline on triage/thumbnail/page classification.
- **Validation:** eval harness.

### Task 13.4: Train aKriti Small/Core adapters

- **Location:** `training/runs/akriti_small_core/`
- **Description:** Adapt open weights using owned data and PEFT.
- **Complexity:** 9
- **Dependencies:** Sprint 8, Task 13.2
- **Acceptance Criteria:**
  - Improves over base on aKriti eval buckets.
  - Exports to at least one local runtime.
- **Validation:** eval harness and local inference smoke test.

## Testing strategy

Per sprint:
- schema/unit tests for data contracts
- fixture-based tests for ingestion/export/search
- visual manual checks for overlays
- evaluation metrics for OCR/layout/table/chart
- integration tests for API flows
- runtime smoke tests per backend

Release gates:
- no output without provenance
- no high-impact edit without preview/approval
- schema validity above threshold
- table/chart benchmarks separated from text OCR
- exact-search regression tests for dates/names/amounts/citations

## Potential risks and gotchas

| Risk | Mitigation |
|---|---|
| research drift | docs and decision log are canonical |
| overclaiming "our model" too early | use "aKriti-adapted" until owned checkpoints exist |
| VLM hallucinated OCR | provenance, exact checks, low-confidence review |
| restoration hallucination | original immutable, restored derived, disagreement review |
| vector search misses exact facts | exact/layout search first |
| LibreOffice integration complexity | start with local API and action schema before C++ work |
| low-compute mismatch | model tiers and runtime capability detection |
| license contamination | research ledger and model registry must track license |
| court workflow safety | Vinti separate, AI assists structure only |

## Rollback plan

Docs:
- revert individual docs if architecture changes.

Code:
- keep modules behind interfaces.
- disable experimental runtime/model backends by config.
- keep deterministic ingestion independent from VLM modules.
- keep external baseline experiments outside final product path.

## Next immediate commits

1. Commit docs canonicalization.
2. Add `core/akriti_doc` schema package.
3. Add basic validators/exporters.
4. Add born-digital PDF ingestion.
5. Add visual workbench fixture viewer.

## Research References

This doc is connected to the numbered research bibliography in `docs/akriti-research-reference-index.md`. Those references are engineering anchors for aKriti-owned implementation; they are not product dependencies. Only open weights may enter model lineage, and only with manifest provenance.
