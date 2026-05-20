# aKriti Research/Docs Session Handoff — 2026-05-20

**Status:** Session handoff and provenance note  
**Purpose:** Preserve what changed during the post-restart aKriti documentation/research pass so the next implementation session does not lose context or violate locked decisions.

## 1. Session objective

```text
Use newly installed aKriti and Orchestra research skills after restart to drive the next aKriti documentation/research pass, adding deeper granular details where useful while preserving locked ownership, VLM-first, local-first, and verification-first decisions.
```

## 2. Skills/patterns used

| Skill/pattern | How it shaped the work |
|---|---|
| `akriti-vlm-project` | Kept decisions grounded in aKriti, Kriti, LibreOffice, FilterTube, Vinti, aKritiDoc, ownership, and local-first constraints. |
| `akriti-experiment-loop` | Forced research ideas into hypothesis, benchmark slice, fixed budget, metric, stop condition, and promotion gate format. |
| Orchestra/autoresearch pattern | Converted the broad research landscape into controlled experiment queues and implementation gates instead of open-ended hype tracking. |
| Orchestra research-manager pattern | Inspired this handoff/provenance note; full `ara/` epilogue can be created later after a bounded code/research run. |

## 3. Major artifacts added in this pass

| Artifact | Purpose |
|---|---|
| `akriti-research-to-experiment-matrix.md` | Maps papers, OCR/VLM references, optimizers, quantization, runtime, diffusion/restoration, and UI ideas into classifications and experiment gates. |
| `akriti-fixture-corpus-and-experiment-cards.md` | Defines fixture bundles, manifest schema, dataset cards, and EXP-001 through EXP-012. |
| `akriti-model-package-manifests.md` | Defines model manifest, runtime package manifest, capability cards, tier expectations, quantization ladder, and release gates. |
| `akriti-repo-scaffold-blueprint.md` | Defines first executable repository scaffold, minimal file tree, dependency graph, and anti-drift rules. |
| `akriti-contract-schema-implementation-spec.md` | Defines executable schema layout, shared definitions, validation modes, invariant checker responsibilities, examples, and schema-phase gate. |
| `akriti-docs-phase-completion-audit.md` | Audits docs-phase readiness and states that the docs/research phase is ready to move into executable scaffold work. |
| `akriti-scaffold-implementation-backlog.md` | Converts the scaffold into SCAFF-001 through SCAFF-012 implementation tickets with dependencies and acceptance criteria. |
| `akriti-glossary-and-naming.md` | Locks naming and wording, including Vinti as a long-term separate downstream project. |

## 4. Locked decisions reaffirmed

| Decision | Current state |
|---|---|
| aKriti is VLM-first | OCR is one owned capability inside document intelligence, not the product identity. |
| aKriti is not a wrapper | External OCR/VLM systems are references, baselines, teachers, or inspiration only. |
| Open weights are allowed | They may initialize, teach, benchmark, or distill; lineage must be stated honestly. |
| aKritiDoc is canonical | Every parser, runtime, UI, evaluator, exporter, and integration writes through or consumes `aKritiDoc`. |
| Local-first is mandatory | Cloud/remote paths require explicit consent and cannot be silent fallback. |
| Verification is mandatory | Provenance, confidence, citations, low-confidence review, and eval gates are core contracts. |
| Diffusion is restoration-only for now | It can enhance degraded scans as derived artifacts; it is not the core parser/reasoner. |
| Vinti is downstream | Vinti is a long-term separate court/legal product based on aKriti, not core aKriti v1. |

## 5. Research classification state

The research landscape now has a routing rule:

```text
research input -> classification -> owned module lane -> benchmark slice -> fixed-budget experiment -> promotion gate
```

Examples:

| Research area | Current classification |
|---|---|
| open-weight base-family candidate/open-weight base-family candidate | prototype as Core/Pro base or teacher candidates, not identity. |
| external OCR specialist, external OCR specialist, external OCR specialist, external OCR/layout toolkit | engineering-reference only, not product dependencies, label sources, verifier sources, or teacher systems. |
| external document-layout reference-style layout UI | adopt as UX/API reference for overlays and review. |
| PEFT/LoRA/QLoRA/DoRA | adopt for first adaptation path. |
| adaptive low-rank training reference, LoRA variants | prototype for adapter efficiency/rank discovery. |
| extreme quantization reference, GGUF, MLX, WebGPU | prototype/adopt depending on runtime lane and evidence. |
| token-bag pretraining, training-only hierarchical attention, neuron-preserving optimizer, tile-wise sparse packing, four-bit pretraining reference | watch or v2/v3 prototype; not v1 blockers. |
| world-model reference/world-model reference/V-world-model reference | prototype only for visual embeddings/triage, not OCR/reasoning core. |
| Diffusion/dLLM | watch/restoration/editing research only; not core parser. |

## 6. Experiment queue state

The first controlled experiments are defined as EXP-001 through EXP-012.

| ID | Topic |
|---|---|
| EXP-001 | deterministic born-digital PDF fast path |
| EXP-002 | OCR/document specialist error taxonomy |
| EXP-003 | open-weight base-family candidate-family grounded VLM teacher/base eval |
| EXP-004 | FilterTube tiny classifier path |
| EXP-005 | restoration/dewarp with entity-drift gates |
| EXP-006 | table structural evaluator |
| EXP-007 | chart understanding as data reconstruction |
| EXP-008 | Indic translation with layout preservation |
| EXP-009 | confidence voting and review routing |
| EXP-010 | local runtime package comparison |
| EXP-011 | LibreOffice selection-grounded action safety |
| EXP-012 | correction capture for safe training data |

These experiments are not commands to run yet. They become runnable only after schemas, fixtures, validators, and eval report formats exist.

## 7. Implementation backlog state

The next executable work is SCAFF-001 through SCAFF-012.

| ID | Work |
|---|---|
| SCAFF-001 | create scaffold directories and README files |
| SCAFF-002 | add shared common schemas |
| SCAFF-003 | add `aKritiDoc` v0 schema |
| SCAFF-004 | add minimal valid `aKritiDoc` fixture |
| SCAFF-005 | add schema validator and invariant checker skeleton |
| SCAFF-006 | add eval report skeleton and first metric definitions |
| SCAFF-007 | add API/job/action/export schemas |
| SCAFF-008 | add registry and runtime package schemas |
| SCAFF-009 | add experimental placeholder model manifests |
| SCAFF-010 | add fake local API job flow over fixture |
| SCAFF-011 | add Workbench static viewer implementation plan |
| SCAFF-012 | add LibreOffice and FilterTube example contracts |

`MODEL-001` is explicitly blocked until scaffold work exists.

## 8. Do-not-violate list for next session

```text
Do not start training before schemas/fixtures/evals exist.
Do not download or commit model weights in scaffold work.
Do not turn external OCR specialist, external OCR specialist, external OCR specialist, external OCR/layout toolkit, or any other reference into final product dependency.
Do not claim open-derived checkpoints are fully owned.
Do not hide low confidence from users.
Do not let diffusion/restoration overwrite source truth.
Do not silently use cloud/remote models.
Do not implement Vinti as aKriti v1 scope.
Do not build chat UI before evidence/provenance display exists.
```

## 9. Recommended next session command path

Start implementation with:

```text
SCAFF-001 -> SCAFF-002 -> SCAFF-003 -> SCAFF-004 -> SCAFF-005
```

That means:

```text
create dirs/readmes
create common JSON Schemas
create aKritiDoc schema
create one minimal fixture
create validator/invariant checker skeleton
```

Only after that should model bake-offs or runtime package experiments begin.

## 10. Current goal status

The documentation/research pass is substantially complete and ready to move into executable scaffold work, but the broader active goal should stay open until either:

```text
A. the user explicitly asks to stop at docs/research readiness, or
B. an implementation scaffold pass proves the contracts with files and examples.
```

This handoff exists to prevent loss of context between those phases.

## 11. Next-agent guide

See `docs/akriti-next-agent-operating-guide.md` for the operating instructions future agents should follow before editing docs, creating scaffold files, evaluating new research, or starting implementation tickets.

## Research References

This doc is connected to the numbered research bibliography in `docs/akriti-research-reference-index.md`. Those references are engineering anchors for aKriti-owned implementation; they are not product dependencies. Only open weights may enter model lineage, and only with manifest provenance.
