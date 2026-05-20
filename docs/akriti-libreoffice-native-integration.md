# aKriti LibreOffice Native Integration

**Status:** Draft implementation spec  
**Date:** 2026-05-20  
**Purpose:** Define how aKriti should integrate natively with LibreOffice while keeping model/runtime logic outside the office core where appropriate.

## 1. Integration principle

LibreOffice integration should be native at the document/UI boundary and decoupled at the model/runtime boundary.

```text
LibreOffice native document model
        |
        v
UNO/C++ sidebar and canvas integration
        |
        v
aKriti local API or library boundary
        |
        v
aKriti Runtime and models
```

## 2. Why not a thin extension-only product

aKriti should not be just a browser-like extension bolted onto LibreOffice.

Required native behaviors:
- understand current document structure.
- read user selection.
- map model output to native edit operations.
- preview edits before applying.
- preserve undo/redo.
- handle Writer/Calc/Impress separately.
- keep user documents local unless explicit remote mode is enabled.

## 3. LibreOffice surfaces

| Surface | Purpose |
|---|---|
| sidebar chat/action pane | ask, translate, rewrite, explain, verify |
| document canvas overlays | show OCR/layout/table/chart regions |
| selection action menu | act on highlighted text, table, image, chart, page |
| review panel | approve/reject generated edits |
| model manager dialog | choose local aKriti model package |
| privacy/status indicator | local/remote/runtime state |

## 4. Writer workflows

Core actions:
- explain selected text.
- translate selection while preserving style.
- rewrite paragraph.
- extract cited facts from document.
- summarize section.
- identify inconsistencies.
- convert scanned inserted image into editable text.
- preserve original and derived text distinction.

Edit flow:

```text
selection -> aKriti request -> derived patch -> preview -> user approval -> native edit -> undo-compatible operation
```

## 5. Calc workflows

Core actions:
- explain formula.
- generate chart from natural language.
- read chart and explain trend.
- extract table from PDF/image into sheet.
- clean messy imported table.
- translate headers/cells.
- detect anomalies.

Calc-specific requirement:
- generated edits must map to cells, ranges, sheets, formulas, and chart objects.

## 6. Impress workflows

Core actions:
- summarize deck.
- rewrite slide copy.
- translate deck while preserving layout.
- describe images/charts.
- generate speaker notes.
- verify data claims against embedded tables/charts.

Impress-specific requirement:
- preserve slide object positions and avoid destructive layout changes by default.

## 7. aKriti request envelope

```json
{
  "host": "libreoffice",
  "app": "writer | calc | impress",
  "document_id": "lo_doc_...",
  "selection": {},
  "operation": "ask | translate | rewrite | extract | verify | apply_edit",
  "content_refs": [],
  "privacy": {
    "local_only": true
  },
  "runtime_preferences": {
    "model_tier": "tiny | small | core | pro",
    "allow_remote": false
  }
}
```

## 8. Edit patch object

```json
{
  "patch_id": "patch_...",
  "target_app": "writer | calc | impress",
  "target_refs": [],
  "operations": [
    {
      "kind": "replace_text | insert_text | update_cell | insert_table | create_chart | add_comment",
      "target": {},
      "value": {},
      "style_policy": "preserve | adapt | explicit",
      "requires_review": true
    }
  ],
  "provenance": {},
  "risk": "low | medium | high"
}
```

## 9. Safety rules

- never apply high-impact edits without preview.
- preserve undo/redo.
- mark derived text and generated translations where needed.
- keep original evidence available.
- for legal/court documents, default to comments/suggestions instead of direct destructive edits.

## 10. Runtime boundary

LibreOffice should not need to know whether a request was served by:
- GGUF.
- MLX.
- ONNX.
- LiteRT.
- remote Pro model.

LibreOffice should know:
- local or remote.
- model tier.
- operation status.
- citations/provenance.
- patch preview.

## 11. ASCII flow

```text
LibreOffice selection
        |
        v
sidebar action
        |
        v
aKriti request envelope
        |
        v
aKriti API/runtime
        |
        v
aKritiDoc / derived patch
        |
        v
preview and approval
        |
        v
native UNO edit
```

## 12. Mermaid flow

```mermaid
flowchart TD
    A["LibreOffice selection"] --> B["Sidebar/action pane"]
    B --> C["aKriti request envelope"]
    C --> D["aKriti local API"]
    D --> E["Runtime selector"]
    E --> F["aKriti model/module"]
    F --> G["aKritiDoc result or edit patch"]
    G --> H["Preview/review UI"]
    H --> I{"User approves?"}
    I -->|"yes"| J["Native UNO edit"]
    I -->|"no"| K["Discard or revise"]
```


## Research References

This doc is connected to the numbered research bibliography in `docs/akriti-research-reference-index.md`. Those references are engineering anchors for aKriti-owned implementation; they are not product dependencies. Only open weights may enter model lineage, and only with manifest provenance.
