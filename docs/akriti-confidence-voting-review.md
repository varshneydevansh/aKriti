# aKriti Confidence, Voting, and Human Review

**Status:** Draft implementation spec  
**Date:** 2026-05-20  
**Purpose:** Define how aKriti handles uncertain/confusing regions through confidence scoring, multi-pass voting, disagreement detection, and user-visible review.

## 1. Core principle

aKriti should not hide uncertainty.

```text
if the system is unsure
  show where
  show why
  show alternatives
  ask for review when needed
```

Low confidence is not a bug by itself. Silent confidence is the bug.

## 2. Where confidence exists

Confidence should be attached at multiple levels:

| Level | Examples |
|---|---|
| page | scan quality, rotation, language/script certainty |
| block | block type, bbox, reading order |
| span | OCR/text certainty, language/script certainty |
| table | table detection, cell structure, merged cells |
| chart | chart type, axes, legend, series extraction |
| image/figure | region type, caption reliability |
| translation | entity preservation, terminology, layout fit |
| restoration | hallucination risk, entity drift |
| answer | citation support, unsupported claims |
| edit patch | destructive risk, target certainty |

## 3. aKritiDoc confidence object

```json
{
  "confidence": {
    "overall": 0.74,
    "text": 0.91,
    "layout": 0.68,
    "reading_order": 0.72,
    "language": 0.83,
    "script": 0.95,
    "source_grounding": 0.88,
    "review_required": true,
    "review_reasons": [
      "layout_disagreement",
      "low_table_structure_confidence"
    ]
  }
}
```

Confidence is not a replacement for provenance. It is a triage signal.

## 4. Voting / multi-pass reading

Voting is used for confusing or high-impact regions.

Candidate voters:
- primary VLM parse.
- deterministic PDF extraction for born-digital documents.
- restored-image reread.
- table-specific reader.
- chart-specific reader.
- text/OCR specialist baseline used as reference.
- smaller verifier model.
- user correction.

Voting output:

```json
{
  "vote_id": "vote_...",
  "target_ref": {
    "page_id": "page_0002",
    "block_id": "blk_..."
  },
  "candidates": [
    {
      "source": "primary_vlm",
      "value": "Rs. 1,50,000",
      "confidence": 0.66
    },
    {
      "source": "restored_reread",
      "value": "Rs. 1,50,000",
      "confidence": 0.81
    },
    {
      "source": "deterministic_pdf",
      "value": "Rs. 1,60,000",
      "confidence": 0.70
    }
  ],
  "decision": "accept | review | reject | abstain",
  "selected_candidate": null,
  "reason": "conflicting_amounts"
}
```

## 5. Voting policy

Use voting when:
- confidence is below threshold.
- multiple parsers disagree.
- source is degraded or restored.
- legal/financial/identity entities are involved.
- table/chart data is extracted.
- an edit patch targets uncertain content.

Do not use voting as blind majority rule.

Decision priority:

```text
source evidence
  >
deterministic extraction when valid
  >
verified multi-pass agreement
  >
model confidence
  >
majority count
```

If voters disagree on a high-impact entity, mark for review instead of forcing a winner.

## 6. Disagreement types

| Type | Example |
|---|---|
| text disagreement | `150000` vs `160000` |
| bbox disagreement | different region selected |
| block type disagreement | table vs paragraph |
| reading order disagreement | multi-column order mismatch |
| language/script disagreement | Hindi vs Marathi, Devanagari vs Latin |
| restoration drift | restored page introduces new token |
| chart disagreement | line chart vs scatter, axis value mismatch |
| table disagreement | split/merged cell mismatch |

## 7. User-visible low-confidence UI

Workbench/LibreOffice must show:
- highlighted low-confidence regions.
- confidence reason.
- alternatives if available.
- source/original view.
- restored/derived view if used.
- accept/reject/correct actions.

UI labels should be plain:

```text
Low confidence: possible table structure issue
Conflicting reads: "1,50,000" vs "1,60,000"
Needs review: restored image changed visible amount
```

Do not show only numeric scores without explanation.

## 8. Review queue item

```json
{
  "review_id": "rev_...",
  "severity": "low | medium | high",
  "reason": "conflicting_amounts",
  "target_ref": {},
  "source_view": {},
  "candidates": [],
  "recommended_action": "user_review",
  "allowed_actions": [
    "accept_candidate",
    "correct_value",
    "mark_unknown",
    "rerun_with_restoration",
    "ignore"
  ]
}
```

## 9. Thresholds

Start conservative:

| Area | Review threshold |
|---|---:|
| OCR text | `< 0.80` |
| table structure | `< 0.85` |
| chart reconstruction | `< 0.85` |
| legal/financial entities | `< 0.95` or any disagreement |
| restoration output | any entity drift |
| edit patch target | `< 0.90` |
| answer citation support | no direct citation |

Thresholds should become learned/calibrated later.

## 10. Training value

User corrections in review queues are high-value data.

With consent, they can become:
- OCR correction examples.
- layout correction examples.
- table/chart labels.
- verifier training data.
- preference pairs.
- hallucination/red-team samples.

## 11. ASCII flow

```text
parse result
    |
    v
confidence check
    |
    +--> high confidence -> normal output
    |
    +--> low/conflict -> multi-pass voting
                          |
                          v
                    disagreement detector
                          |
                          v
                    review queue / abstain
                          |
                          v
                    user correction if needed
```

## 12. Mermaid flow

```mermaid
flowchart TD
    A["Parse result"] --> B["Confidence check"]
    B -->|"high confidence"| C["Normal output"]
    B -->|"low/conflict"| D["Multi-pass voting"]
    D --> E["Disagreement detector"]
    E --> F{"High-impact or unresolved?"}
    F -->|"yes"| G["Review queue"]
    F -->|"no"| H["Accept with warning"]
    G --> I["User correction"]
    I --> J["Optional training event with consent"]
```


## Research References

This doc is connected to the numbered research bibliography in `docs/akriti-research-reference-index.md`. Those references are engineering anchors for aKriti-owned implementation; they are not product dependencies. Only open weights may enter model lineage, and only with manifest provenance.

## 13. On-demand bounded voter loops

Some tasks need more than one pass. aKriti/Vinti should support on-demand bounded voter loops for disputed regions, confusing OCR, Indian-language glyph ambiguity, and high-impact triage questions.

The loop is not infinite reasoning. It is a controlled review process.

```text
disputed region or triage question
        |
        v
initial candidate set
        |
        v
5-7 independent analyzer votes
        |
        v
disagreement detector
        |
        +--> enough agreement -> accept or accept_with_warning
        |
        +--> disagreement -> targeted reread / restoration / critique
                                  |
                                  v
                              final revote
                                  |
                                  v
                  accept | needs_human_review | abstain
```

Rules:

- Use an odd number of analyzer votes when possible: 5 or 7 for high-impact Vinti tasks.
- Majority is not enough for legal, financial, identity, or case-routing decisions.
- Each vote must cite source evidence.
- Each critique must identify what it disagrees with.
- Each loop has a fixed budget and max iteration count.
- Human review is a valid decision, not a failure.
- If restoration changes a name, amount, date, citation, stamp, signature, or case number, mark it for review.

## 14. Analyzer roles for Vinti

Vinti-specific analyzer roles:

| Analyzer | Question |
|---|---|
| case-type analyzer | what kind of case is this? |
| completeness analyzer | is the file complete? |
| readiness analyzer | is it procedurally ready? |
| fast-track analyzer | is it simple enough to fast-track? |
| mediation/ODR analyzer | should it go to mediation or ODR? |
| defect analyzer | does it need defect correction? |
| evidence verifier | which pages support the recommendation? |
| disagreement auditor | which analyzers disagreed and why? |
| human-validation tracker | which human verified it? |

Not every question needs every analyzer. The orchestrator should run only the analyzers needed for the current page, region, or triage decision.

## 15. Glyph and restoration dispute policy

For Indian-language character confusion or degraded scans:

```text
original crop
    |
    +--> direct OCR read
    |
    +--> restored-image reread
    |
    +--> language/script consistency check
    |
    +--> analyzer vote
    |
    v
accepted candidate OR human review
```

Restoration-generated characters are never silently promoted to source truth. They are candidate readings with provenance.

UI must show:

- original crop.
- restored crop if used.
- candidate characters/words.
- analyzer confidence.
- reason for dispute.
- review action.

## 16. Loop trace object

```json
{
  "loop_id": "loop_...",
  "target_ref": {},
  "question": "case_type | file_complete | ready | fast_track | mediation_odr | defect_correction | evidence_support | glyph_dispute",
  "max_iterations": 3,
  "iterations_used": 2,
  "votes": [],
  "critiques": [],
  "restoration_artifacts": [],
  "decision": "accepted | accepted_with_warning | needs_human_review | abstained | restoration_failed | insufficient_evidence",
  "ledger_hash_eligible": true,
  "human_review_required": true
}
```

## 17. Grounding and precision-stability review policy

Reference anchors: [30], [31], [32].

For high-stakes document work, the confidence system must evaluate both content and location:

```text
claim text
    |
    +--> source text confidence
    +--> source region grounding confidence
    +--> bbox/polygon validity
    +--> runtime precision stability
    +--> analyzer agreement
    v
accept | accept_with_warning | needs_region_review | needs_human_review
```

Additional review triggers:

- the extracted claim has no grounded page region.
- two plausible regions support different readings.
- bbox/polygon is invalid or unstable across runtime variants.
- low-precision output changes OCR text, table structure, formula symbols, names, dates, amounts, citations, stamps, signatures, or Vinti triage bucket.
- generation loops, repeats, or emits malformed JSON.

The UI label should distinguish:

```text
needs_text_review       -> the content is uncertain
needs_region_review     -> the content may be right but the evidence location is uncertain
needs_precision_review  -> runtime/quantization changed the result
needs_human_review      -> legal/financial/case-routing risk remains unresolved
```

## 18. Feedback loop policy

Reference anchor: [37].

Voting and review corrections should improve the system, but never by mutating production behavior silently.

Allowed flow:

```text
review item
    -> human correction
    -> feedback event
    -> failure taxonomy
    -> offline harness/adaptor proposal
    -> eval gate
    -> approved release
```

Every analyzer vote and triage output must record:

- model version.
- harness version.
- schema version.
- analyzer versions.
- thresholds used.
- retry/re-read/restoration policy.
- review policy.

Blocked:

- changing thresholds in production without a harness version.
- auto-training from new cases.
- allowing feedback agent proposals to bypass evaluation.
- hiding disagreement because a newer harness would have decided differently.

## 19. Analyzer terminology

Reference anchor: [38].

Use precise terminology:

```text
aKriti Analyzer
  reusable document-level analyzer:
  OCR confidence, language ambiguity, layout, grounding, table/chart, entity, restoration drift, precision stability.

Kriti Orchestrator
  generic local tool/action coordinator:
  chooses tools, validates schemas, requests review, proposes safe actions.

Vinti Analyzer Pack
  court-logistics analyzer set:
  case type, file completeness, procedural readiness, fast-track, ADR/ODR, defects, evidence support, human validation, ledger readiness.
```

Voting rules:

- Generic document confidence comes from aKriti analyzers.
- Court-routing votes come from the Vinti Analyzer Pack.
- The Vinti Analyzer Pack must not re-implement OCR/layout/grounding; it consumes aKritiDoc and aKriti analyzer outputs.
- If aKriti reports unresolved text, language, grounding, restoration, or precision ambiguity, Vinti cannot emit high-confidence routing.
- Do not expose raw chain-of-thought. Expose structured vote reasons, evidence refs, disagreement summaries, and review state.
