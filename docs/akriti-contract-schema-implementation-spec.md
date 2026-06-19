# aKriti Contract Schema Implementation Spec

**Status:** Draft implementation-control spec, created 2026-05-20  
**Purpose:** Define how the prose contracts for `aKritiDoc`, API jobs, module IO, edit patches, review items, exports, model manifests, and runtime packages become machine-checkable schemas.

This document operationalizes:

- `docs/akriti-akritidoc-schema-v0.md`
- `docs/akriti-api-job-lifecycle-and-errors.md`
- `docs/akriti-owned-module-interface-contracts.md`
- `docs/akriti-export-conversion-edit-contracts.md`
- `docs/akriti-model-package-manifests.md`
- `docs/akriti-repo-scaffold-blueprint.md`

## 1. Contract principle

Every serious aKriti artifact must be validatable before it is trusted.

```text
candidate output
      |
      v
schema validation
      |
      v
invariant checks
      |
      v
provenance/confidence checks
      |
      v
usable by UI/API/eval/export/runtime
```

Invalid schema output is not a partial success. It is a failed artifact until converted or repaired.

## 2. Schema package layout

Implement these files first:

```text
schemas/
  README.md
  common/
    id.schema.json
    timestamp.schema.json
    bbox.schema.json
    source-ref.schema.json
    confidence.schema.json
    provenance.schema.json
    error.schema.json
    privacy.schema.json
    runtime-stats.schema.json
  akritidoc/
    v0.schema.json
    block.schema.json
    span.schema.json
    table.schema.json
    chart.schema.json
    visual-artifact.schema.json
    derived-artifact.schema.json
    verification.schema.json
  api/
    request-envelope.schema.json
    response-envelope.schema.json
    job.schema.json
    progress-event.schema.json
    error.schema.json
  modules/
    module-request.schema.json
    module-response.schema.json
    akritidoc-patch.schema.json
  actions/
    edit-patch.schema.json
    edit-operation.schema.json
    review-item.schema.json
  exports/
    export-request.schema.json
    export-artifact.schema.json
    conversion-warning.schema.json
  registry/
    model-manifest.schema.json
    runtime-package.schema.json
    capability-card.schema.json
```

## 3. Schema conventions

Use JSON Schema as the first source of executable truth.

Rules:

| Rule | Requirement |
|---|---|
| `$schema` | use JSON Schema draft 2020-12 unless tooling forces otherwise |
| `$id` | stable URI-style IDs such as `https://akriti.local/schemas/akritidoc/v0.schema.json` |
| version | every top-level object must include `schema_version` |
| unknown fields | reject by default; allow only inside explicit `metadata` objects |
| coordinates | every bbox declares coordinate `space` and `page_id` |
| confidence | use numeric `0..1`, plus optional label when useful |
| provenance | required for source-derived blocks/spans/tables/charts/images |
| derived content | must point back to source refs |
| high-risk action | must include review/approval requirement |

Recommended strict default:

```json
{
  "additionalProperties": false
}
```

Exception:

```text
metadata objects may allow arbitrary keys, but must not carry source-critical truth.
```

## 4. Validation modes

Use three modes instead of one global mode.

| Mode | Use | Behavior |
|---|---|---|
| `strict` | release gates, exports, registry approval | fail on missing required fields, invalid refs, unknown schema-critical fields |
| `partial` | async jobs and page-level partial results | allow incomplete pages/artifacts if marked partial |
| `experimental` | research runs and candidate conversions | allow candidate-specific metadata but still require IDs/provenance/confidence where applicable |

Important:

```text
experimental mode is not a product permission. It only allows research artifacts to be captured for analysis.
```

## 5. Shared definitions

### 5.1 IDs

Use stable prefixes so artifacts are readable in logs.

| Object | Prefix |
|---|---|
| document | `doc_` |
| source | `src_` |
| page | `page_` |
| block | `blk_` |
| span | `span_` |
| table | `tbl_` |
| cell | `cell_` |
| chart | `chart_` |
| artifact | `artifact_` |
| derived artifact | `derived_` |
| job | `job_` |
| event | `evt_` |
| request | `req_` |
| patch | `patch_` |
| review item | `review_` |
| model | `akriti-` or `kriti-` |
| runtime package | `{model_id}-{runtime}-{quantization}-{platform}` |

### 5.2 Source ref

Every source-grounded object should be able to point to its origin.

```json
{
  "source_id": "src_...",
  "page_id": "page_0001",
  "block_id": "blk_...",
  "span_id": "span_...",
  "table_id": null,
  "cell_id": null,
  "chart_id": null,
  "bbox": {
    "x": 0.1,
    "y": 0.2,
    "w": 0.3,
    "h": 0.1,
    "space": "normalized",
    "page_id": "page_0001"
  }
}
```

### 5.3 Confidence

Confidence must separate different uncertainty types.

```json
{
  "overall": 0.82,
  "text": 0.91,
  "layout": 0.75,
  "structure": 0.68,
  "grounding": 0.95,
  "label": "high | medium | low | needs_review | abstained",
  "reasons": ["low_contrast", "conflicting_table_structure"]
}
```

Rule:

```text
If confidence affects user trust, it must be visible to Workbench/LibreOffice UI, not hidden in debug logs.
```

### 5.4 Provenance

```json
{
  "created_by": {
    "module": "aKriti Text Reader",
    "model_id": "...",
    "package_id": "...",
    "runtime": "..."
  },
  "source_refs": [],
  "derived_from": [],
  "created_at": "2026-05-20T00:00:00Z",
  "method": "deterministic | model | human_correction | teacher_generated | synthetic_ground_truth",
  "verification_status": "unverified | checked | failed | human_approved"
}
```

## 6. `aKritiDoc` schema responsibilities

`schemas/akritidoc/v0.schema.json` should be the top-level schema composed from smaller files.

Required top-level fields:

```text
schema_version
document_id
source
pages
derived_artifacts
operations
verification
metadata
```

Optional but standardized fields:

```text
global_entities
global_tables
global_charts
indexes
exports
```

Hard invariants:

| Invariant | Validation location |
|---|---|
| page IDs are unique | invariant checker |
| block IDs are unique within document | invariant checker |
| every bbox page exists | schema + invariant checker |
| reading order references existing blocks | invariant checker |
| source text is not overwritten by derived text | patch/invariant checker |
| derived artifacts reference source refs | schema + invariant checker |
| visual blocks are first-class | block enum + required visual artifact refs |
| low-confidence items can become review items | confidence/review schema link |

Use schema for shape. Use invariant checker for cross-reference validity that JSON Schema handles poorly.

## 7. Block schema responsibilities

Block types:

```text
text
title
list
table
chart
image
figure
formula
signature
stamp
form_field
footer
header
unknown
```

Required fields:

```text
block_id
type
bbox
source_refs
confidence
provenance
metadata
```

Conditional fields:

| If type is | Then require |
|---|---|
| `text`, `title`, `list`, `footer`, `header` | `spans` |
| `table` | `table_ref` or embedded table object |
| `chart` | `chart_ref` or embedded chart object |
| `image`, `figure`, `signature`, `stamp` | `artifact_ref` |
| `unknown` | `review_reason` or low confidence reason |

## 8. Table and chart schemas

Tables and charts must not collapse into prose.

Table required fields:

```text
table_id
block_id
bbox
rows
cols
cells
confidence
provenance
```

Cell required fields:

```text
cell_id
row
col
row_span
col_span
bbox
text
spans
confidence
source_refs
```

Chart required fields:

```text
chart_id
block_id
chart_type
bbox
axes
legends
series
confidence
provenance
```

Chart rule:

```text
A generated chart caption is a derived artifact. Reconstructed chart data is a structured chart field or extracted-data artifact. They are not the same claim.
```

## 9. API schemas

### 9.1 Request envelope

```json
{
  "request_id": "req_...",
  "operation": "parse | ask | search | restore | translate | export | apply_edit | verify",
  "input": {},
  "privacy": {},
  "runtime_preferences": {},
  "idempotency_key": "..."
}
```

Required privacy fields:

```text
local_only
allow_remote
allow_training_reuse
consent_class
```

Default:

```text
local_only = true
allow_remote = false
allow_training_reuse = false
```

### 9.2 Job schema

Required fields:

```text
job_id
kind
status
created_at
updated_at
request
progress
artifacts
review_items
errors
```

Job invariant:

```text
A completed parse job must point to a valid aKritiDoc artifact.
```

### 9.3 Progress event schema

Required fields:

```text
event_id
job_id
type
timestamp
payload
```

Allowed event types:

```text
progress
artifact
warning
review_item
partial_result
complete
failed
cancelled
```

## 10. Module request/response schemas

Module request required fields:

```text
request_id
module
operation
document_ref
target_refs
artifacts
constraints
runtime_preferences
```

Allowed modules:

```text
layout
text
table
chart
image
restoration
translation
reasoning
runtime
```

Module response required fields:

```text
request_id
module
status
akritidoc_patch
derived_artifacts
confidence
verification
review_items
runtime
errors
```

Allowed statuses:

```text
success
partial
failed
abstained
needs_review
```

Hard rule:

```text
A module may abstain. It may not invent source evidence to avoid abstaining.
```

## 11. Patch schemas

### 11.1 aKritiDoc patch

Used internally to merge module output into document state.

Required fields:

```text
patch_id
target_document_id
base_document_version
ops
risk_level
requires_review
provenance
```

Allowed ops:

```text
add
update
link
mark_review
add_derived_artifact
```

Avoid `delete` in v0 except for explicitly derived/candidate objects. Source evidence deletion should be impossible in normal patches.

### 11.2 Edit patch

Used for Workbench/LibreOffice/native edits.

Required fields:

```text
patch_id
target
operations
risk
preview_required
inverse_patch
provenance
```

High-risk edit rule:

```text
If risk is high, preview_required must be true and requires_user_approval must be true for every operation.
```

## 12. Review item schema

Review item required fields:

```text
review_id
type
target_refs
reason
severity
confidence
candidates
recommended_action
created_by
status
```

Review types:

```text
low_confidence_text
conflicting_votes
table_structure_ambiguous
chart_data_uncertain
translation_entity_change
restoration_entity_drift
unsafe_edit
unsupported_claim
privacy_consent_needed
```

Rule:

```text
If the system knows it is uncertain, that uncertainty must become a review item or an explicit abstention.
```

## 13. Export schemas

Export request required fields:

```text
export_id
document_id
source_version
target_format
selection_refs
options
```

Export artifact required fields:

```text
artifact_id
kind
format
path
sha256
source_document_id
source_version
created_at
warnings
provenance
```

Export invariant:

```text
High-stakes exports must preserve citations/provenance or emit warnings that provenance was omitted.
```

## 14. Registry schemas

Model manifest and runtime package schemas are release-control contracts.

Model manifest required groups:

```text
identity
lineage
training
capabilities
languages_scripts
limits
confidence_policy
eval_evidence
runtime_packages
release
```

Runtime package required groups:

```text
package identity
runtime and format
quantization
platforms
files and checksums
runtime requirements
measured performance
quality delta
approved/blocked surfaces
```

Registry invariant:

```text
No package can be `default-local` without eval evidence and at least one runtime package card.
```

## 15. Invariant checker responsibilities

JSON Schema alone is insufficient. Add an invariant checker after schema validation.

Checks:

| Check | Applies to |
|---|---|
| unique IDs | all documents/manifests |
| ref exists | pages, blocks, spans, tables, charts, artifacts |
| bbox page exists | blocks/spans/cells/charts/artifacts |
| reading order refs valid blocks | page reading order |
| derived artifacts have source refs | derived content |
| source text not overwritten | patches |
| confidence threshold creates review item | low-confidence outputs |
| remote use obeys privacy flags | API requests/jobs |
| model package has release evidence | registry |
| Vinti is downstream-only | integration metadata and docs |

## 16. Schema-to-code generation

Preferred order:

```text
JSON Schema
  -> TypeScript types for Workbench
  -> Python models for eval/API tooling
  -> Rust/C++ structs only after contract stabilizes
```

Do not hand-maintain divergent schemas in each language.

If codegen is painful, keep JSON Schema authoritative and write thin validators per language.

## 17. Minimal example requirements

Every schema group must include one valid example and one invalid example.

Required examples:

```text
schemas/akritidoc/examples/minimal-page.akritidoc.json
schemas/api/examples/parse-job.complete.json
schemas/modules/examples/text-reader.partial.json
schemas/actions/examples/libreoffice-comment-patch.json
schemas/registry/examples/experimental-core-manifest.json
schemas/registry/examples/gguf-runtime-package.json
```

Invalid examples:

```text
missing provenance
invalid bbox
hidden low confidence
source overwrite patch
model manifest without lineage
runtime package without checksum
```

## 18. Implementation order

```text
1. common schemas
2. aKritiDoc top-level + minimal example
3. invariant checker skeleton
4. API envelope/job/error schemas
5. module request/response/patch schemas
6. review item and confidence schemas
7. export/edit patch schemas
8. model/runtime package schemas
9. example corpus and invalid examples
10. schema-to-code generation or thin validators
```

## 19. Completion gate for schema phase

The schema phase is complete when:

```text
[ ] common schemas exist
[ ] aKritiDoc v0 schema exists
[ ] one valid minimal aKritiDoc fixture validates
[ ] at least three invalid examples fail for the expected reason
[ ] API job schema validates a fake complete parse job
[ ] module response schema validates one partial/abstained output
[ ] edit patch schema enforces preview for high-risk edits
[ ] registry schema validates only experimental placeholder manifests
[ ] invariant checker catches at least ref/bbox/source-overwrite issues
```

Do not promote a model/runtime package before this gate exists.

## 20. Why this protects the aKriti direction

This schema layer preserves the locked decisions:

| Locked decision | Schema protection |
|---|---|
| VLM-first | model outputs become structured document intelligence, not OCR-only text |
| not a wrapper | external systems can only produce candidate artifacts under aKriti schemas |
| local-first | privacy flags and runtime package manifests expose cloud/local behavior |
| verification-first | provenance, confidence, review items, and citations are required contracts |
| owned model path | lineage and weights-origin fields prevent dishonest ownership claims |
| Vinti downstream | high-stakes/legal fields can exist without making Vinti core v1 scope |
```

## Research References

This doc is connected to the numbered research bibliography in `docs/akriti-research-reference-index.md`. Those references are engineering anchors for aKriti-owned implementation; they are not product dependencies. Only open weights may enter model lineage, and only with manifest provenance.

## Schema-Guided Extraction and Formula Schemas

Reference anchors: [26], [27].

Add these schema files to the first implementation batch:

```text
schemas/
  extraction/
    extraction-schema.schema.json
    extracted-field.schema.json
    extraction-result.schema.json
    field-evidence.schema.json
  math/
    formula.schema.json
    symbol-confidence.schema.json
    math-conversion-request.schema.json
    math-conversion-result.schema.json
```

Required invariants:

- Every extracted field has a declared type.
- Every extracted field has source evidence unless explicitly marked `user_supplied`.
- Every normalized value preserves the original verbatim source value.
- Formula conversion keeps the original region, source format, target format, and conversion warnings.
- Invalid JSON extraction output is a failed artifact, not a partial success.
- Low-confidence formula symbols must be reviewable in the UI.
