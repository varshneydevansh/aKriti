# aKriti Next-Agent Operating Guide

**Status:** Operating guide for future provider-native code-agent and research-agent sessions, created 2026-05-20  
**Purpose:** Keep future agents aligned with the aKriti direction while moving from documentation/research into executable scaffold and later model experiments.

This guide is for any agent working on aKriti, Kriti, LibreOffice integration, FilterTube local VLM features, or Vinti downstream planning.

## 1. First rule

Do not improvise the project direction from scratch.

Start from the canonical docs, then act.

```text
read locked decisions
  -> read current phase handoff
  -> read scaffold backlog
  -> implement the next accepted ticket
  -> update docs/ledger only when facts change
```

## 2. First files to read

Use the smallest set needed.

| Task | Read first |
|---|---|
| any aKriti work | `docs/akriti-decision-log.md` |
| architecture question | `docs/akriti-master-architecture.md` |
| current continuity | `docs/akriti-research-docs-session-handoff-2026-05-20.md` |
| next implementation task | `docs/akriti-scaffold-implementation-backlog.md` |
| schema work | `docs/akriti-contract-schema-implementation-spec.md` |
| fixture/eval work | `docs/akriti-fixture-corpus-and-experiment-cards.md` |
| model/package work | `docs/akriti-model-package-manifests.md` |
| research/paper/model evaluation | `docs/akriti-research-to-experiment-matrix.md` and `docs/akriti-research-ledger.md` |
| Vinti mention | `docs/akriti-vinti-court-downstream-spec.md` and `docs/akriti-glossary-and-naming.md` |

Read `LOCAL_DOCS` historical files only if canonical docs are insufficient or the user asks for past discussion context.

## 3. Skill routing

Use installed skills deliberately.

| Work | Skills/patterns |
|---|---|
| aKriti project decisions | `akriti-vlm-project` |
| fixed-budget experiments | `akriti-experiment-loop` |
| broad multi-hypothesis research | `autoresearch`, but only after executable evals exist |
| research provenance epilogue | `ara-research-manager`, only at session closeout |
| fine-tuning/adapters | `peft-fine-tuning`, `axolotl`, `unsloth`, `fine-tuning-with-trl` |
| distributed/cloud training | `huggingface-accelerate`, `pytorch-fsdp2`, `deepspeed`, `distributed-llm-pretraining-torchtitan` |
| quantization/local runtime | `gguf-quantization`, `llama-cpp`, `awq-quantization`, `gptq`, `hqq-quantization`, `tensorrt-llm` |
| serving/eval | `serving-llms-vllm`, `sglang`, `evaluating-llms-harness` |
| retrieval | `sentence-transformers`, `faiss`, `qdrant-vector-search` |
| VLM baselines | `llava`, `blip-2-vision-language`, `clip`, `segment-anything-model` |
| UI/design | `ui-ux-superstack`, `design-taste-frontend`, `gstack-plan-design-review` when reviewing rendered UI |

Do not use a skill just because it exists. Use it when it advances the current ticket.

## 4. Locked invariants

Future agents must preserve these.

```text
1. aKriti is VLM-first document intelligence, not OCR-only.
2. aKriti is not a wrapper around external OCR/VLM systems.
3. Open weights can initialize, teach, benchmark, or distill, but lineage must be honest.
4. aKritiDoc is the canonical representation.
5. Local-first and offline-first behavior is default.
6. Verification, provenance, confidence, and review are product contracts.
7. Diffusion/restoration cannot overwrite source truth.
8. Vinti is a long-term separate downstream project based on aKriti.
9. LibreOffice is a first-class native integration target.
10. FilterTube uses tiny/local paths first, not default 3B browser inference.
```

## 5. Current phase

The project is at:

```text
docs/research phase complete enough to start executable scaffold work
```

The next practical sequence is:

```text
SCAFF-001 -> SCAFF-002 -> SCAFF-003 -> SCAFF-004 -> SCAFF-005
```

Meaning:

```text
create scaffold directories/readmes
create common schemas
create aKritiDoc schema
create minimal fixture
create validator/invariant checker skeleton
```

## 6. Blocked work

Do not start these yet:

| Blocked work | Blocker |
|---|---|
| model training | schemas, fixtures, validators, eval report skeleton do not exist yet |
| model download/package | registry/runtime package manifests do not exist yet |
| open-weight base-family candidate/open-weight edge-model reference/VLM bake-off | candidate outputs cannot yet be validated under aKritiDoc |
| OCR specialist integration | external systems are only baselines/teachers, not product dependencies |
| LibreOffice C++/UNO implementation | request/patch examples and fake API flow should exist first |
| FilterTube WebGPU model | thumbnail fixture records and tiny baseline contract should exist first |
| Vinti implementation | Vinti is downstream after aKriti APIs/model packages stabilize |
| chat-first Workbench UI | evidence/provenance/review display must come before chat polish |

## 7. Research input handling

When the user brings a new paper, repo, tweet, model release, or optimization trick, classify it first.

Use this format:

```text
classification: adopt-now | prototype | watch | reject
module lane: Layout/Text/Table/Chart/Image/Restoration/Translation/Kriti/Runtime/Data/Eval
product surface: Workbench | LibreOffice | FilterTube | Vinti later | training only
benchmark slice: exact fixture/eval needed
promotion gate: what must be measured before roadmap impact
```

If the idea cannot produce one of these, it stays as watch/reference:

```text
reproducible benchmark result
new required eval metric
safer product invariant
smaller/faster local runtime package
better owned-module training target
clearer user-facing verification workflow
```

## 8. Ownership wording rules

Use precise wording.

Allowed:

```text
open-derived aKriti adapter
open-weight base candidate
aKriti-owned schema/API/runtime/eval/product layer
aKriti distilled student with mixed lineage
aKriti-owned module checkpoint, only when true
```

Avoid:

```text
fully ours, if based on open weights and not yet distilled/owned
using external OCR specialist/external OCR specialist as our OCR engine
aKriti is just OCR
aKriti is the court product
Vinti is part of v1
```

## 9. Documentation update rules

Update docs when one of these changes:

| Change | Update file |
|---|---|
| locked decision changes | `akriti-decision-log.md` |
| research classification changes | `akriti-research-ledger.md` and possibly `akriti-research-to-experiment-matrix.md` |
| new experiment planned | `akriti-fixture-corpus-and-experiment-cards.md` or experiment protocol |
| new scaffold task | `akriti-scaffold-implementation-backlog.md` |
| schema boundary changes | `akriti-contract-schema-implementation-spec.md` |
| model package rule changes | `akriti-model-package-manifests.md` |
| Vinti boundary changes | `akriti-vinti-court-downstream-spec.md` and `akriti-glossary-and-naming.md` |

Do not update every doc for minor wording. Keep docs coherent, not noisy.

## 10. Coding behavior for next phase

When implementation begins:

```text
1. implement one SCAFF ticket at a time.
2. create small files with explicit README boundaries.
3. avoid generated bulk code until schemas settle.
4. keep examples tiny and inspectable.
5. do not run expensive tests or downloads unless user asks.
6. do not commit model weights, private docs, or large artifacts.
7. preserve user changes and do not revert unrelated work.
```

First good commit:

```text
SCAFF-001: scaffold directories and README files
```

Second good commit:

```text
SCAFF-002: common schemas
```

Do not combine scaffold, model experiments, UI, and runtime work in one commit.

## 11. Safety and privacy defaults

Default behavior:

```text
local_only = true
allow_remote = false
allow_training_reuse = false
source documents are not training data
private docs are not fixtures
cloud teacher use requires explicit consent
```

For high-stakes/legal/court-like documents:

```text
exact search first
citations required
abstention allowed
low confidence shown
human review required for risky outputs
```

## 12. Vinti boundary reminder

Vinti is important, but not current core scope.

Correct framing:

```text
Vinti is a long-term separate downstream court/legal product based on aKriti.
```

Current aKriti work should support Vinti later by building:

```text
aKritiDoc
evidence/provenance
exact-first retrieval
review queues
safe edits
model package registry
local-first runtime
```

Do not hardcode court-specific assumptions into core aKriti v1 APIs.

## 13. End-of-session handoff rule

At the end of a meaningful session, write a short handoff if any of these changed:

```text
decisions
new docs
new scaffold files
new experiment results
research classifications
model/package status
Vinti boundary
```

Use `ara-research-manager` style only as an epilogue, not during active execution.

## Current Policy Update: External Research Boundary

When extending the docs, do not describe named external OCR, VLM, video, and reasoning projects as components of aKriti. Describe them as engineering references only. aKriti may study what they do, then implement the capability in-house. Only open weights may enter the model lineage, and that requires manifest provenance.

Detailed named research notes are kept outside the project repo.

## Research References

This doc is connected to the numbered research bibliography in `docs/akriti-research-reference-index.md`. Those references are engineering anchors for aKriti-owned implementation; they are not product dependencies. Only open weights may enter model lineage, and only with manifest provenance.
