# aKriti Fixture Corpus and Experiment Cards

**Status:** Draft implementation-control spec, created 2026-05-20  
**Purpose:** Define the first concrete fixture corpus and experiment cards that turn the research matrix into measurable aKriti work.

This document connects:

- `docs/akriti-research-to-experiment-matrix.md`
- `docs/akriti-data-engine-and-synthetic-documents.md`
- `docs/akriti-evaluation-harness.md`
- `docs/akriti-akritidoc-schema-v0.md`

It is intentionally practical. The goal is to make the first aKriti experiments small, reproducible, local-first, and hard to fool.

## 1. Fixture principle

A fixture is not just a sample PDF or image. A fixture is a source artifact plus expected structured truth.

```text
source file/page/image
    |
    v
rendered page artifact
    |
    v
ground-truth or expected aKritiDoc slice
    |
    v
metric-specific expected outputs
    |
    v
experiment result report
```

A fixture is valid only if it can answer these questions:

| Question | Required answer |
|---|---|
| What is the source artifact? | file path, checksum, license, consent class |
| What should aKriti produce? | expected `aKritiDoc` blocks, spans, tables, charts, images, translations, or actions |
| What metric reads it? | OCR, layout, table, chart, translation, runtime, edit, retrieval, confidence |
| What proves success? | threshold plus provenance and safety checks |
| What must never happen? | hallucinated text, lost source coordinates, hidden uncertainty, cloud leak |

## 2. Proposed fixture repository layout

This is a spec, not yet an implemented directory.

```text
fixtures/
  README.md
  manifests/
    fixture-index.jsonl
    dataset-cards/
      akriti-golden-25.md
      akriti-scan-30.md
      akriti-tablechart-45.md
      akriti-indic-50.md
      akriti-filtertube-500.md
  raw/
    pdf/
    images/
    office/
    thumbnails/
  rendered/
    pages/
    regions/
    thumbnails/
  akritidoc/
    expected/
    candidate/
  expected/
    text/
    layout/
    tables/
    charts/
    translations/
    edits/
    retrieval/
    confidence/
  reports/
    experiment-runs/
    error-taxonomy/
    regression/
```

Do not put private user documents here unless there is explicit consent and a dataset card marks the allowed use.

## 3. Fixture manifest schema

Each fixture should have one JSONL record in `fixtures/manifests/fixture-index.jsonl`.

```json
{
  "fixture_id": "akriti_golden_25_0001",
  "dataset_id": "akriti-golden-25",
  "source": {
    "path": "fixtures/raw/pdf/report_0001.pdf",
    "sha256": "...",
    "mime_type": "application/pdf",
    "license": "synthetic | public-domain | consented-private | unknown",
    "consent_class": "train_allowed | eval_only | local_only | no_reuse"
  },
  "rendered": {
    "pages": ["fixtures/rendered/pages/report_0001_p001.png"],
    "dpi": 200
  },
  "truth": {
    "akritidoc": "fixtures/akritidoc/expected/report_0001.akritidoc.json",
    "text": "fixtures/expected/text/report_0001.txt",
    "layout": "fixtures/expected/layout/report_0001.layout.json",
    "tables": [],
    "charts": []
  },
  "tags": ["born-digital", "report", "english", "table", "figure"],
  "split": "dev | heldout | redteam",
  "intended_experiments": ["EXP-001", "EXP-006"],
  "known_failure_modes": ["multi-column reading order", "caption near figure"]
}
```

## 4. Dataset cards

Every dataset card should include:

```markdown
# Dataset Card: {dataset_id}

- Purpose:
- Source policy:
- Consent/license posture:
- Split policy:
- Included document families:
- Languages/scripts:
- Ground-truth layers:
- Metrics supported:
- Known limitations:
- Not allowed for:
```

Required rule:

```text
If source policy or consent is unclear, the fixture is eval-only or local-only. It is not training data.
```

## 5. First fixture bundles

| Bundle | Purpose | Minimum size | Source type | Primary experiments |
|---|---:|---:|---|---|
| `akriti-golden-25` | born-digital deterministic fast path | 25 docs | synthetic/public PDFs | EXP-001, EXP-006, EXP-007 |
| `akriti-scan-30` | OCR/layout/restoration baseline | 30 pages | scanned public/synthetic images | EXP-002, EXP-005, EXP-009 |
| `akriti-vlm-grounded-50` | page-grounded VLM teacher/base evaluation | 50 pages | mixed rendered docs | EXP-003 |
| `akriti-tablechart-45` | table/chart structural eval | 45 regions/pages | synthetic plus public docs | EXP-006, EXP-007 |
| `akriti-indic-50` | Indic OCR and translation | 50 blocks/pages | synthetic/public Indic docs | EXP-002, EXP-008, EXP-009 |
| `akriti-filtertube-500` | local thumbnail/title filtering | 500 items | thumbnail/title/channel metadata | EXP-004 |
| `akriti-libreoffice-actions-40` | selection-grounded edit/action safety | 40 ODT/ODS cases | generated office docs | EXP-011 |
| `akriti-corrections-25` | correction capture and dataset eligibility | 25 reviewed fixtures | synthetic correction sessions | EXP-012 |

## 6. EXP-001 fixture card: born-digital fast path

Hypothesis:

```text
A deterministic born-digital PDF path can produce useful aKritiDoc fixtures before model training.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Documents | 25 PDFs |
| Families | reports, invoices, forms, tables, figures, bilingual reports |
| Ground truth | text blocks, reading order, page numbers, headings, simple tables, image blocks |
| Metrics | text coverage, block provenance, reading order, schema validity |
| Runtime target | CPU-only local path |
| Safety check | no cloud use, no model dependency |

Promotion gate:

```text
95%+ expected text extraction coverage with page/block provenance and valid aKritiDoc output.
```

## 7. EXP-002 fixture card: OCR/document specialist bake-off

Hypothesis:

```text
External OCR/document specialists are useful as teachers and baselines because they reveal the error taxonomy aKriti must own.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Pages | 30 scanned/rendered pages |
| Scripts | English, Hindi, at least one additional Indic script, Hinglish/code-mixed, numeric-heavy pages |
| Layouts | single-column, multi-column, table-heavy, form-like, stamp/signature near text |
| Ground truth | line text, span text, script labels, reading order, bbox per line/block |
| Metrics | CER, WER, Indic CER, reading order accuracy, hallucinated span count |
| Safety check | external systems are compared only; they are not product dependencies |

Promotion gate:

```text
A complete error taxonomy exists and aKriti Text/Layout Reader targets are measurable against it.
```

## 8. EXP-003 fixture card: open-weight base-family candidate-family grounded VLM teacher/base eval

Hypothesis:

```text
A open-weight base-family candidate-family VLM can act as a Core/Pro base or teacher only if it produces grounded document outputs under the aKritiDoc contract.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Pages | 50 mixed pages |
| Tasks | extract fields, answer page-grounded questions, describe figures, reconstruct small tables, identify low-confidence regions |
| Expected outputs | answer text plus page/block/bbox evidence |
| Metrics | grounded answer accuracy, unsupported claim rate, confidence calibration, latency |
| Local target | Mac M4 and RTX 2060-class quantized/runtime path when feasible |
| Safety check | unsupported answers must abstain or mark low confidence |

Promotion gate:

```text
Beats at least one alternate open-weight candidate on grounded accuracy without worse hallucination or runtime behavior.
```

## 9. EXP-004 fixture card: FilterTube tiny classifier path

Hypothesis:

```text
Most FilterTube decisions can be handled by rules plus tiny text/vision classifiers, with VLM reserved for ambiguity.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Items | 500 video-like records |
| Inputs | title, channel, thumbnail, optional transcript snippet |
| Labels | allow, block, downrank, ambiguous, needs-user-rule |
| Metrics | false-block rate, false-allow rate, ambiguous routing, thumbnail latency |
| Runtime target | browser WebGPU/WASM, CPU fallback |
| Safety check | user rules override model guesses |

Promotion gate:

```text
80%+ decisions handled by tiny path while keeping false-block rate below the chosen user-safety threshold.
```

## 10. EXP-005 fixture card: restoration/dewarp with entity-drift gates

Hypothesis:

```text
Restoration helps only when it improves readability without changing factual text.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Pages | 40 degraded scans |
| Degradations | blur, skew, curvature, compression, shadows, low contrast |
| Ground truth | original text, entity spans, numbers, dates, visual artifact map |
| Metrics | OCR delta, line preservation, entity drift, number/date drift |
| Runtime target | CPU/GPU local preprocessing |
| Safety check | restored image is always a derived artifact; original remains source of truth |

Promotion gate:

```text
CER improves while entity, number, and date drift remain below the hard threshold.
```

## 11. EXP-006 fixture card: table structural evaluator

Hypothesis:

```text
Table quality must be measured structurally before model training can be trusted.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Tables | 20 to start, expanding to 45 table/chart bundle |
| Cases | simple grids, merged cells, missing borders, multi-line cells, numeric-heavy tables |
| Ground truth | cell bboxes, row/column graph, merged-cell spans, CSV/HTML expected output |
| Metrics | table F1, cell IoU, adjacency accuracy, CSV round-trip, cell text CER |
| Safety check | table captions and notes remain separate from cell data |

Promotion gate:

```text
Metrics catch structural failures that plain OCR CER misses.
```

## 12. EXP-007 fixture card: chart understanding as data reconstruction

Hypothesis:

```text
Chart understanding should be evaluated by reconstructed data, not attractive captions.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Charts | 25 to start |
| Types | bar, line, scatter, pie, stacked bar, histogram where possible |
| Ground truth | chart type, title, axis labels, tick values, legend, series, data table |
| Metrics | chart type accuracy, axis/legend extraction, series reconstruction error, chart QA |
| Safety check | caption-only output is insufficient |

Promotion gate:

```text
Extracted data/axis/legend metrics expose whether the model actually understood the chart.
```

## 13. EXP-008 fixture card: Indic translation with layout preservation

Hypothesis:

```text
aKriti translation must preserve meaning, terminology, script fidelity, and layout.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Blocks/pages | 50 |
| Scripts | Hindi plus at least one additional Indic script; include Hinglish/code-mixed samples |
| Ground truth | source spans, translation reference, terminology glossary, entity list |
| Metrics | chrF/BLEU as rough checks, terminology consistency, entity preservation, layout score |
| Safety check | native-script fidelity is required; Hinglish is augmentation, not replacement |

Promotion gate:

```text
Translation passes terminology, entity, script, and layout checks, not only fluent text checks.
```

## 14. EXP-009 fixture card: confidence voting and review routing

Hypothesis:

```text
Confidence voting should expose uncertainty to users instead of hiding ambiguity.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Cases | ambiguous OCR, confusable scripts, table ambiguity, chart ambiguity, translation alternatives |
| Ground truth | accepted answer, alternate plausible answers, review reason |
| Metrics | review recall, review precision, unresolved ambiguity rate, reviewer burden |
| Safety check | if candidates are ungrounded, output abstains instead of voting among guesses |

Promotion gate:

```text
Low-confidence queue catches real errors without overwhelming the reviewer.
```

## 15. EXP-010 fixture card: local runtime package comparison

Hypothesis:

```text
GGUF and MLX cover the first desktop-local runtime needs before heavier runtime lanes are necessary.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Models | Tiny/Small/Core candidate packages when available |
| Tasks | text extraction assist, page QA, thumbnail/page triage, translation block |
| Metrics | RAM, VRAM, cold start, tokens/sec, pages/sec, failure/OOM rate |
| Targets | CPU-only desktop, RTX 2060 6GB, Mac M4 24GB |
| Safety check | quality cannot be sacrificed silently for quantization speed |

Promotion gate:

```text
Runtime card clearly states package size, memory, latency, quality deltas, and failure modes.
```

## 16. EXP-011 fixture card: LibreOffice selection-grounded action safety

Hypothesis:

```text
LibreOffice actions are safer when grounded to explicit selections and previewable patches.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Cases | 40 ODT/ODS documents |
| Actions | summarize selection, translate selection, rewrite paragraph, extract table, create chart, explain image |
| Ground truth | selected range, allowed operation, expected patch, rollback state |
| Metrics | patch correctness, selection fidelity, destructive-edit prevention, user approval correctness |
| Safety check | document-wide edits require explicit user approval |

Promotion gate:

```text
Preview/apply/rollback flow prevents destructive edits and preserves source provenance.
```

## 17. EXP-012 fixture card: correction capture for safe training data

Hypothesis:

```text
aKritiDoc corrections can become high-value training data only when consent, provenance, and eligibility are explicit.
```

Fixture requirements:

| Requirement | Value |
|---|---|
| Sessions | 25 correction sessions |
| Corrections | text span, bbox adjustment, table cell repair, chart value repair, translation correction |
| Ground truth | before, after, reviewer id class, consent class, reason |
| Metrics | correction replay validity, dataset eligibility, provenance completeness |
| Safety check | private docs are no-reuse by default |

Promotion gate:

```text
Every correction has provenance, consent class, and training eligibility metadata.
```

## 18. Cross-experiment invariants

These invariants apply to every experiment.

| Invariant | Failure condition |
|---|---|
| Valid `aKritiDoc` | output cannot be parsed or lacks required provenance |
| Original source retained | restoration/export overwrites or replaces source truth |
| Confidence exposed | uncertain output is presented as certain |
| Local-first respected | experiment requires cloud for a user-local path without explicit remote fallback label |
| External systems bounded | external OCR/VLM becomes a hidden product dependency |
| Vinti boundary respected | court-specific assumptions leak into core aKriti v1 APIs |

## 19. Experiment report format

Each run should emit a report under `fixtures/reports/experiment-runs/`.

```markdown
# Experiment Report: EXP-{number}-{slug}

- Date:
- Candidate:
- Baseline:
- Fixture bundle:
- Device/runtime:
- Budget:
- Metrics:
- Quality result:
- Provenance result:
- Runtime result:
- Safety result:
- Error taxonomy additions:
- Decision: keep | park | reject
- Follow-up:
```

## 20. Initial build order

Start in this order:

```text
1. fixture manifest schema
2. akriti-golden-25 born-digital sample set
3. minimal aKritiDoc expected labels
4. EXP-001 deterministic extraction report
5. akriti-scan-30 OCR/restoration set
6. EXP-002 error taxonomy report
7. table/chart fixture bundle
8. confidence-voting ambiguity set
9. runtime package cards
10. LibreOffice action fixtures
```

Reason:

```text
Data and evals must exist before model claims. Otherwise every model choice becomes taste and hype instead of engineering evidence.
```

## 21. Done criteria for fixture phase

The fixture phase is done when:

| Criterion | Evidence |
|---|---|
| At least three fixture bundles exist | manifest records and dataset cards |
| `aKritiDoc` labels validate | schema validation report |
| EXP-001 and EXP-002 have baseline reports | reports with metrics and error taxonomy |
| No private data leakage path exists | consent class present for every source |
| Low-confidence review examples exist | EXP-009 fixture cases |
| Runtime package reporting is defined | EXP-010 report template filled for first candidate |

Do not claim model progress before these exist.

## Research References

This doc is connected to the numbered research bibliography in `docs/akriti-research-reference-index.md`. Those references are engineering anchors for aKriti-owned implementation; they are not product dependencies. Only open weights may enter model lineage, and only with manifest provenance.
