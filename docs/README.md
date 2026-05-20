# Docs

This folder contains project documentation that supports implementation, onboarding, and operation.

Canonical aKriti planning/spec documents:
- `docs/akriti-master-architecture.md` - locked product, model-family, module, runtime, and integration architecture.
- `docs/akriti-api-capability-map.md` - user-facing capabilities, APIs, schemas, and ASCII/Mermaid flow diagrams.
- `docs/akriti-research-ledger.md` - research/model/tool ledger distilled from `LOCAL_DOCS`, provider-native code agent harness chats, and deep-research reports.
- `docs/akriti-decision-log.md` - locked decisions, rejected paths, research-only lanes, and ownership rules.
- `docs/akriti-implementation-plan.md` - phased sprint plan with atomic, committable tasks.

Planned operational documents:
- `docs/runbook.md`
- `docs/contributing.md`
- `docs/security-and-privacy.md`

## Agent skills and provider-native code agent harness tooling

- See `docs/akriti-skills-map.md` for the aKriti-specific skills, installed research skills, duplicate-skill cleanup, and cross-device sync command.

## Granular engineering plans

- `docs/akriti-training-and-distillation-plan.md` defines the staged path from open-weight bases to adapters, distilled students, and owned aKriti modules.
- `docs/akriti-runtime-deployment-matrix.md` maps aKriti Tiny/Small/Core/Pro onto GGUF, MLX, ONNX, LiteRT, Core ML, WebGPU, and server runtimes.
- `docs/akriti-evaluation-harness.md` defines document-native metrics for OCR, layout, tables, charts, translation, retrieval, editing, runtime, and safety.
- `docs/akriti-experiment-loop.md` adapts the Karpathy/autoresearch fixed-budget loop to aKriti model and document-intelligence experiments.

## Operational implementation specs

- `docs/akriti-akritidoc-schema-v0.md` defines the first detailed `aKritiDoc` object model, including source, pages, blocks, spans, tables, charts, derived artifacts, provenance, verification, and operations.
- `docs/akriti-retrieval-grounding-plan.md` defines exact-first retrieval, semantic/vector retrieval, citations, abstention, and grounding rules.
- `docs/akriti-libreoffice-native-integration.md` defines the native LibreOffice sidebar/canvas/action flow, request envelope, edit patch shape, and safety boundaries.
- `docs/akriti-filtertube-local-vlm-plan.md` defines the local-first FilterTube thumbnail/title semantic filtering path using aKriti Tiny/Small.

## Data, restoration, and release governance

- `docs/akriti-restoration-diffusion-module.md` defines diffusion/restoration as a derived-artifact support lane with hallucination, entity-drift, and provenance checks.
- `docs/akriti-data-engine-and-synthetic-documents.md` defines the synthetic document/data engine, dataset versioning, privacy rules, teacher-data verification, and dataset-to-model mapping.
- `docs/akriti-model-registry-release-gates.md` defines model IDs, package manifests, release stages, promotion gates, capability cards, and rollback policy.

## Product, language, privacy, and review UX

- `docs/akriti-multilingual-indic-translation-plan.md` defines native Indic-script fidelity, Hinglish/code-mixed handling, tokenizer policy, and layout-preserving translation.
- `docs/akriti-workbench-ui-product-spec.md` defines the Workbench as a document review cockpit with overlays, grounded chat/actions, verification queues, and correction capture.
- `docs/akriti-security-privacy-local-first.md` defines local-first processing, consent boundaries, remote fallback rules, artifact storage, and high-stakes safety mode.
- `docs/akriti-confidence-voting-review.md` defines confidence scoring, multi-pass voting, disagreement detection, low-confidence UI, and human review queues.

## Reasoning/action and downstream products

- `docs/akriti-kriti-reasoning-action-module.md` defines Kriti as the typed reasoning/action layer with constrained tool calls, action envelopes, risk levels, and auditable results.
- `docs/akriti-vinti-court-downstream-spec.md` defines Vinti as a downstream court/legal document product built on aKriti, with strict citation, review, abstention, and high-stakes safety behavior.

## Module contracts

- `docs/akriti-owned-module-interface-contracts.md` defines shared request/response/patch contracts for aKriti Layout/Text/Table/Chart/Image/Restoration/Translation/Kriti/Runtime modules.

## Candidate bake-offs and hardware experiments

- `docs/akriti-baseline-bakeoff-protocol.md` defines how open VLM/LLM bases, OCR/document references, deterministic extractors, adapters, owned modules, and runtime packages compete under the same `aKritiDoc` contract.
- `docs/akriti-hardware-experiment-plan.md` defines what to run on RTX 2060, Mac M4, CPU-only machines, browser/WebGPU, and cloud GPUs, with fixed budgets and hardware report templates.

## API lifecycle, exports, and edits

- `docs/akriti-api-job-lifecycle-and-errors.md` defines async jobs, progress events, partial artifacts, errors, retries, cancellation, idempotency, and stale edit handling.
- `docs/akriti-export-conversion-edit-contracts.md` defines PDF/DOCX/ODT/ODS/CSV/HTML/JSON exports, export artifacts, conversion warnings, and native edit patch operations.

## Implementation bridge

- `docs/akriti-repository-implementation-map.md` maps the spec set into proposed repo packages, services, apps, runtime adapters, evals, experiments, and integrations.
- `docs/akriti-first-milestone-roadmap.md` defines the first implementation milestones: schemas, fixtures, eval harness, API job skeleton, Workbench viewer, deterministic fast path, bake-off, aKriti Tiny, LibreOffice proof, and first model-assisted parser.

## Coverage and traceability

- `docs/akriti-spec-coverage-traceability.md` maps locked objectives to spec files, first implementation artifacts, invariants, remaining open design gaps, and docs-phase readiness criteria.

## Research maintenance

- `docs/akriti-research-maintenance-provenance.md` defines how future papers, repos, models, experiments, decisions, and session closeouts should update the research ledger, decision log, traceability map, and roadmap without causing roadmap drift.

## Glossary and naming

- `docs/akriti-glossary-and-naming.md` defines canonical terms, ownership wording, OCR terminology, model tier names, confidence/evidence terms, runtime terms, research status words, and the rule that Vinti is a long-term separate downstream project based on aKriti.

## Research-to-experiment control

- `docs/akriti-research-to-experiment-matrix.md` maps papers, model releases, OCR/VLM references, optimizers, quantization methods, runtime ideas, diffusion/restoration ideas, and UI concepts into `adopt-now`, `prototype`, `watch`, or `reject`, with required aKriti benchmark slices and promotion gates.

## Fixture corpus and experiment cards

- `docs/akriti-fixture-corpus-and-experiment-cards.md` defines the first concrete fixture bundles, manifest schema, dataset cards, and EXP-001 through EXP-012 experiment cards required before model/runtime claims are promoted.

## Model package manifests

- `docs/akriti-model-package-manifests.md` defines the manifest schema, runtime package card, capability card, tier-specific expectations, quantization ladder, and release checks required before aKriti Tiny/Small/Core/Pro/Kriti packages can be used by product surfaces.

## Repository scaffold blueprint

- `docs/akriti-repo-scaffold-blueprint.md` defines the first concrete implementation scaffold for `schemas/`, `fixtures/`, `evals/`, `registry/`, `packages/`, `services/`, `runtimes/`, `apps/`, `integrations/`, and `tools/`, including the first file tree, dependency graph, acceptance checklist, and anti-drift rules.

## Contract schema implementation

- `docs/akriti-contract-schema-implementation-spec.md` defines how `aKritiDoc`, API jobs, module requests/responses, edit patches, review items, exports, model manifests, and runtime packages become machine-checkable schemas with shared definitions, validation modes, invariant checks, examples, and implementation order.

## Docs-phase audit

- `docs/akriti-docs-phase-completion-audit.md` audits the current documentation/research pass against the active objective, confirms coverage for ownership, VLM-first, local-first, verification-first, and Vinti downstream boundaries, and lists remaining executable scaffold gaps before model claims.

## Scaffold implementation backlog

- `docs/akriti-scaffold-implementation-backlog.md` converts the docs/research phase into concrete SCAFF-001 through SCAFF-012 implementation tickets with dependencies, acceptance criteria, and gates that block premature model claims.

## Research/docs session handoff

- `docs/akriti-research-docs-session-handoff-2026-05-20.md` summarizes the post-restart documentation/research pass, newly added artifacts, locked decisions, experiment queue, scaffold backlog, do-not-violate rules, and recommended next implementation path.

## Next-agent operating guide

- `docs/akriti-next-agent-operating-guide.md` tells future provider-native code-agent and research-agent what to read first, which skills to use, which invariants to preserve, what work is blocked, how to classify new research, and how to start the executable scaffold phase without drifting into model demos or wrappers.

## System diagrams and feature map

- `docs/akriti-system-diagrams-and-feature-map.md` consolidates aKriti capabilities, end-to-end document flow, aKritiDoc center, model family, runtime deployment, verification/confidence loop, product surfaces, LibreOffice, FilterTube, Vinti downstream flow, training ownership path, and first executable scaffold path.
