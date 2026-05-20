# aKriti Docs-Phase Completion Audit

**Status:** Current-state audit, created 2026-05-20  
**Purpose:** Check whether the documentation/research pass is strong enough to move into executable scaffold work while preserving the locked aKriti boundaries.

This is not a claim that the full project is complete. It is a readiness audit for the docs/research phase.

## 1. Audit scope

Objective being audited:

```text
Use the newly installed aKriti and Orchestra research skills after restart to drive the next aKriti documentation/research pass, adding deeper granular details where useful while preserving the locked ownership, VLM-first, local-first, and verification-first decisions.
```

The audit checks whether the current docs set covers:

| Requirement | Meaning |
|---|---|
| installed skills used | newly installed aKriti/research skills shaped the pass |
| research inputs captured | prior research ideas are ledgered and classified |
| granular detail added | specs moved beyond high-level roadmap into contracts, fixtures, manifests, scaffolds |
| ownership preserved | open weights/reference systems are not described as final product dependencies |
| VLM-first preserved | OCR remains a capability inside document intelligence, not the product identity |
| local-first preserved | runtime/device/deployment docs protect offline/local use |
| verification-first preserved | provenance, confidence, low-confidence review, evals, and gates are explicit |
| Vinti boundary preserved | Vinti is long-term downstream, not aKriti v1 scope |

## 2. Current docs inventory evidence

Current inventory includes these active docs:

```text
akriti-master-architecture.md
akriti-decision-log.md
akriti-research-ledger.md
akriti-api-capability-map.md
akriti-implementation-plan.md
akriti-training-and-distillation-plan.md
akriti-runtime-deployment-matrix.md
akriti-evaluation-harness.md
akriti-experiment-loop.md
akriti-akritidoc-schema-v0.md
akriti-api-job-lifecycle-and-errors.md
akriti-owned-module-interface-contracts.md
akriti-export-conversion-edit-contracts.md
akriti-retrieval-grounding-plan.md
akriti-libreoffice-native-integration.md
akriti-filtertube-local-vlm-plan.md
akriti-restoration-diffusion-module.md
akriti-data-engine-and-synthetic-documents.md
akriti-model-registry-release-gates.md
akriti-multilingual-indic-translation-plan.md
akriti-workbench-ui-product-spec.md
akriti-security-privacy-local-first.md
akriti-confidence-voting-review.md
akriti-kriti-reasoning-action-module.md
akriti-vinti-court-downstream-spec.md
akriti-owned-module-interface-contracts.md
akriti-baseline-bakeoff-protocol.md
akriti-hardware-experiment-plan.md
akriti-repository-implementation-map.md
akriti-first-milestone-roadmap.md
akriti-spec-coverage-traceability.md
akriti-research-maintenance-provenance.md
akriti-glossary-and-naming.md
akriti-research-to-experiment-matrix.md
akriti-fixture-corpus-and-experiment-cards.md
akriti-model-package-manifests.md
akriti-repo-scaffold-blueprint.md
akriti-contract-schema-implementation-spec.md
akriti-docs-phase-completion-audit.md
```

## 3. Coverage decision table

| Requirement | Evidence | Audit result |
|---|---|---|
| Skills used after restart | `akriti-vlm-project`, `akriti-experiment-loop`, and Orchestra research-skill patterns shaped the research-to-experiment matrix and fixture plan. | covered for docs pass |
| Prior research classified | `akriti-research-ledger.md` and `akriti-research-to-experiment-matrix.md` classify open-weight base-family candidate, open-weight edge-model reference/LiteRT, OCR references, PEFT, extreme quantization reference, world-model reference, diffusion, tile-wise sparse packing, token-bag pretraining, training-only hierarchical attention, neuron-preserving optimizer, runtime lanes, and UI references. | covered |
| Experiment discipline added | `akriti-experiment-loop.md`, `akriti-research-to-experiment-matrix.md`, and `akriti-fixture-corpus-and-experiment-cards.md` require hypothesis, benchmark slice, fixed budget, metrics, stop condition, and promotion gate. | covered |
| Data/fixture detail added | `akriti-data-engine-and-synthetic-documents.md` plus `akriti-fixture-corpus-and-experiment-cards.md` define fixture bundles, manifest records, dataset cards, EXP-001 through EXP-012. | covered |
| Model package detail added | `akriti-model-registry-release-gates.md` plus `akriti-model-package-manifests.md` define lineage, capability cards, runtime package cards, release gates, and quantization ladder. | covered |
| Schema implementation detail added | `akriti-akritidoc-schema-v0.md` plus `akriti-contract-schema-implementation-spec.md` define schema layout, shared definitions, validation modes, invariant checks, valid/invalid examples. | covered |
| Repo scaffold detail added | `akriti-repository-implementation-map.md`, `akriti-first-milestone-roadmap.md`, and `akriti-repo-scaffold-blueprint.md` define directories, minimal file tree, build order, do-not-create-yet list, and acceptance checklist. | covered |
| Ownership preserved | `akriti-decision-log.md`, `akriti-training-and-distillation-plan.md`, `akriti-research-ledger.md`, `akriti-model-package-manifests.md`, and `akriti-glossary-and-naming.md` require honest open-weight lineage and reject wrapper identity. | covered |
| VLM-first preserved | `akriti-master-architecture.md`, `akriti-decision-log.md`, `akriti-owned-module-interface-contracts.md`, and `akriti-research-to-experiment-matrix.md` keep OCR as one module inside document intelligence. | covered |
| Local-first preserved | `akriti-security-privacy-local-first.md`, `akriti-runtime-deployment-matrix.md`, `akriti-hardware-experiment-plan.md`, `akriti-model-package-manifests.md`, and `akriti-repo-scaffold-blueprint.md` define local runtimes and privacy defaults. | covered |
| Verification-first preserved | `akriti-evaluation-harness.md`, `akriti-confidence-voting-review.md`, `akriti-retrieval-grounding-plan.md`, `akriti-contract-schema-implementation-spec.md`, and `akriti-fixture-corpus-and-experiment-cards.md` enforce provenance/confidence/review gates. | covered |
| Vinti boundary preserved | `akriti-vinti-court-downstream-spec.md`, `akriti-glossary-and-naming.md`, `akriti-spec-coverage-traceability.md`, and `akriti-repo-scaffold-blueprint.md` state Vinti is a long-term separate downstream project. | covered |

## 4. Remaining gaps before implementation

These are not docs-phase blockers, but they are required before serious model claims.

| Gap | Required next artifact |
|---|---|
| Docs are not yet executable | create `schemas/` files from `akriti-contract-schema-implementation-spec.md` |
| Fixture bundles do not yet exist | create `fixtures/` scaffold and first minimal `aKritiDoc` fixture |
| Eval harness is not yet runnable | create `evals/` skeleton with schema-validity and text/bbox metrics |
| Registry manifests are not yet files | create experimental placeholder manifests under `registry/models/` |
| Runtime packages are only specified | create adapter contract docs and no-op runtime adapter stubs |
| Workbench is only specified | create static fixture viewer after fixture exists |
| LibreOffice integration is only specified | create request/patch examples before C++/UNO work |
| FilterTube model path is only specified | create thumbnail/title fixture records before WebGPU prototype |
| ARA provenance is not materialized | optionally create `ara/` epilogue after a bounded research/code session |

## 5. Docs-phase readiness verdict

Verdict:

```text
The docs/research phase is ready to move into executable scaffold work.
```

Reason:

```text
The current documentation now defines the architecture, locked decisions, research classification, experiment discipline, fixture corpus, model packaging, runtime deployment, schema implementation, and scaffold blueprint required to start code without relying on hype or vague model claims.
```

Important limitation:

```text
This does not prove aKriti itself works. It proves the research/docs pass is sufficiently specified to start implementing schemas, fixtures, evals, registry manifests, and local service scaffolding.
```

## 6. Recommended next action

Start executable scaffold work in this order:

```text
1. create top-level scaffold directories with README files
2. implement `schemas/common/*` and `schemas/akritidoc/v0.schema.json`
3. add one minimal valid `aKritiDoc` fixture
4. add invalid examples for provenance, bbox, source overwrite, and missing lineage
5. add a validator CLI or script
6. add placeholder experimental model manifests
7. add eval report format and schema-validity metric stub
```

## 7. Goal-status decision

Do not mark the broader active goal complete yet if the expected end state includes implementation scaffolding.

Mark the docs/research pass as ready for code only when:

```text
[ ] current docs are committed or intentionally left uncommitted
[ ] README index points to all active docs
[ ] scaffold blueprint is accepted
[ ] no known contradiction exists around ownership, VLM-first, local-first, verification-first, or Vinti boundary
```

The current audit supports moving forward, not stopping.

## 8. Scaffold backlog handoff

See `docs/akriti-scaffold-implementation-backlog.md` for the concrete SCAFF-001 through SCAFF-012 tickets that implement the next step recommended by this audit.

## Research References

This doc is connected to the numbered research bibliography in `docs/akriti-research-reference-index.md`. Those references are engineering anchors for aKriti-owned implementation; they are not product dependencies. Only open weights may enter model lineage, and only with manifest provenance.
