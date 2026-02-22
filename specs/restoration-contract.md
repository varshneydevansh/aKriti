# aKriti Restoration Contract (v1)

Version: `1.0`  
Date: `22 Feb 2026`  
Scope: VLM-only repository (`aKriti`)  

## Purpose

Define how image/text restoration is used in `aKriti` as a **tool**, not a truth source.  
All restoration is non-destructive and provenance-tracked.

## 1. Core policy

1. Restoration never replaces originals.
2. Restoration output is tagged as a derived artifact (`source_type`).
3. Any parse/qa result must track which artifact generated it.
4. Restore-assisted outputs are accepted only if:
   - validator gates pass, and
   - agreement/consistency improves or remains stable, and
   - confidence policy allows auto-accept.
5. If uncertain, escalate to user review queue.

## 2. Artifact chain model

- `original` (immutable, uploaded source)
- `rendered` (canonical raster/page render)
- `restored_det` (deterministic variant, e.g., deskew/denoise/contrast)
- `restored_tair_like` (optional TAIR-style variant)
- `restored_diffusion_assist` (optional, assisted/preview-only variant)
- `crop__{block_id}` (derived region crop from any parent artifact)

Each artifact record:

```json
{
  "artifact_id": "string",
  "artifact_type": "original|rendered|restored_det|restored_tair_like|restored_diffusion_assist|crop",
  "parent_artifact_id": "optional string",
  "page_index": 0,
  "region": null,
  "generator": "deskew-denoise-v1|tair_like|lucidflux_assist",
  "params": {},
  "created_at": "2026-02-22T00:00:00Z",
  "input_hash": "sha256:...",
  "output_hash": "sha256:..."
}
```

## 3. API contract additions

### `POST /v1/restore`

Input:
- `job_id`
- optional `page_index`
- optional `block_id` or `region` (normalized bbox)
- `restore_modes` (array):
  - `deskew`
  - `denoise`
  - `deblur`
  - `contrast_adjust`
  - `threshold`
  - `sharpen`
- optional `variant`:
  - `deterministic_only` (default),
  - `tair_like`
  - `lucidflux_assist`
- optional `auto_rerun` (`true|false`)
- optional `auto_rerun_mode` (`balanced|accurate`)

Output:
- `restored_artifact_ids[]`
- `rendered_artifacts` metadata
- optional `rerun_job_id`

### `POST /v1/parse` (v1 extension)

- new parser setting fields:
  - `parser_settings.restore_policy`:
    - `never`,
    - `on_low_confidence` (default),
    - `always`,
    - `manual_only`
  - `parser_settings.restore_modes` (array from above)
  - `parser_settings.selective_targets`:
    - `all|text|tables|images|signatures|marginalia`
  - `parser_settings.selection_scope`:
    - `full_document|page_list|block_ids|bbox_regions`

### `POST /v1/verify-block`

- optional field: `use_restore=true|false`
- optional: `restore_hint` (strategy + region bounds + max restores)

## 4. Verification tie-ins

When restore is triggered (auto/manual), verification must log:
- candidate source artifact,
- before/after text/score deltas,
- parser route used.

Restoration-assisted rerun output is accepted only if:
- strict validators pass,
- final confidence > acceptance threshold,
- agreement score does not decrease versus best prior route unless manually overridden.

Otherwise:
- mark `needs_review=true`,
- keep original and restored candidates as alternatives,
- preserve both in history.

## 5. Diffusion usage policy

- `restored_diffusion_assist` is assistant-only.
- Diffusion artifacts must never auto-publish to final outputs.
- UI must label as “assistive preview (not source of truth)”.

## 6. User-visible behavior (UI)

- Restore toggle:
  - `Restore Full Page`
  - `Restore Region`
- Region restore command for uncertain blocks or selected canvas region.
- Result panel must show:
  - chosen source artifact,
  - restore operations used,
  - diff summary vs parent block.

## 7. Evaluation gates

- Track and alert deltas:
  - `cer_delta`, `wer_delta`, `validator_pass_delta`, `agreement_delta`
- Block-level metric set:
  - base parse vs restored-assisted parse improvement,
  - regression guard to prevent over-correction.

Acceptance rule:
- Restore feature is “healthy” only if restored-assisted runs improve fidelity or confidence without regressions on the benchmark set.

## 8. Security and audit

- All restore operations and outputs must be immutable logs.
- Keep provenance for chain-of-custody.
- No original overwrites.

## 9. Rollout sequence

1. v1: deterministic restore modes only (`deskew/denoise/...`).
2. v1.1: optional `tair_like` behind feature flag + approval gate.
3. v2: diffusion-assisted restore preview only, explicit opt-in UI warning.
