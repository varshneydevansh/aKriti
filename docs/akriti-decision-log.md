# aKriti Decision Log

**Status:** Locked decisions as of 2026-05-20

## 1. Locked decisions

### D001: aKriti is VLM-first, not OCR-first

Decision:

```text
aKriti is a document VLM/model-family platform with OCR/text reading as one owned capability.
```

Reason:
- OCR alone cannot handle layout, charts, tables, visual grounding, transformations, and reasoning.
- VLM alone still needs validation and structured document contracts.

### D002: aKriti is not a wrapper

Decision:

```text
No external OCR/VLM system is a final product dependency.
```

Open weights may be used as initialization or raw model material with honest lineage. Public projects, OCR/VLM systems, papers, repos, model cards, and demos may be studied for engineering ideas only. Shipped modules, labels, verifiers, runtimes, APIs, and product behavior must be aKriti-owned or implemented in-house.

### D003: aKritiDoc is the canonical primitive

Decision:

```text
Every parser, model, verifier, UI, and exporter writes through aKritiDoc.
```

Reason:
- prevents multiple truths
- enables provenance
- enables deterministic validation
- enables LibreOffice-native edits
- enables Vinti/court evidence flows

### D004: open-weight base-family candidate/open-weight base-family candidate are candidate bases, not identity

Decision:

```text
Evaluate open-weight base-family candidate now and open-weight base-family candidate when stable/open.
Do not hardcode aKriti to open-weight base-family candidate.
```

Reason:
- open-weight base-family candidate currently looks strong, but base models change quickly.
- The system moat is schema, data, evals, runtime, UI, and integration.

### D005: Diffusion is restoration-only for now

Decision:

```text
Use diffusion/rectification for image quality restoration only.
Do not use diffusion as the core parser/reasoning model.
```

Reason:
- restoration can improve readability
- diffusion can hallucinate text-like strokes
- legal/document evidence needs original-preserving provenance

### D006: Exact search is first-class

Decision:

```text
Use exact/layout/entity search before vector search for document QA.
```

Reason:
- dates, amounts, names, sections, citations, and table cells need exactness.
- vector-only RAG can miss critical details.

### D007: Verification is mandatory

Decision:

```text
Every model output must pass schema, provenance, confidence, deterministic checks, or user approval.
```

Reason:
- LLM/VLM outputs are stochastic.
- High-stakes docs require auditability.

### D008: LibreOffice is a first-class native target

Decision:

```text
LibreOffice integration should be native C++/UNO/sidebar/canvas oriented, not merely an extension wrapper.
```

Reason:
- user has LibreOffice core experience
- document-native edits matter
- upstream-compatible architecture should respect LibreOffice internals

### D009: Product is API-first

Decision:

```text
aKriti must expose stable local APIs so any UI/software can plug into it.
```

Targets:
- Workbench
- LibreOffice
- FilterTube
- Vinti
- desktop/mobile/browser apps

### D010: Tokenizer and Hinglish policy

Decision:

```text
Do not swap tokenizers inside pretrained external VLM/LLM bases in v1.
For owned student models, evaluate a grapheme-aware Unigram tokenizer with byte fallback.
Use Hinglish as augmentation/alignment data, not as a replacement for native-script fidelity.
```

Reason:
- pretrained model tokenizers are part of the learned distribution and changing them early creates avoidable instability.
- Indic document intelligence needs native-script fidelity, not only romanized code-mixed handling.
- a custom tokenizer is justified only when it improves measured OCR/text, translation, compression, or runtime outcomes.

### D011: Benchmark and claim lock

Decision:

```text
No component, model, parser, runtime, or release claim should be promoted as "better" without measurements.
```

Required evidence:
- CER/WER and Indic-script CER
- structure/layout accuracy
- grounding and provenance coverage
- reading-order accuracy
- table and chart reconstruction quality
- confidence calibration
- restoration deltas against original scans
- Indic and code-mixed stress tests
- latency, RAM/VRAM, package size, and CPU/GPU behavior for local targets

### D012: Visual blocks are first-class document artifacts

Decision:

```text
Images, figures, plots, charts, signatures, stamps, diagrams, and embedded visual regions are source artifacts in aKritiDoc.
Generated captions/descriptions are derived interpretations and must stay separate from source text.
```

Required metadata:
- bbox and page coordinates
- provenance back to original file/page/region
- artifact type
- extraction confidence
- optional generated caption/description
- optional chart/table reconstruction linked to the source visual region

Reason:
- LibreOffice, PDFs, court documents, and reports often carry meaning in non-text regions.
- treating visuals as first-class blocks enables chart QA, figure extraction, evidence review, and safe native edits.

### D013: Ownership wording must be precise

Decision:

```text
If base weights start from open weights, say so.
Do not claim parser weights are fully ours until the checkpoint/module is actually owned.
It is correct to say the aKriti system, schema, API, runtime packaging, evals, and integration layer are ours.
```

Reason:
- this protects technical honesty.
- it separates product ownership from model-weight provenance.
- it keeps the long-term owned-model path clear without overstating v1.

## 2. Rejected or delayed paths

| Path | Status | Reason |
|---|---|---|
| wrapper around named external OCR/layout systems | rejected for final product | violates ownership |
| OCR-only product | rejected | insufficient for document intelligence |
| vector-RAG-only document chat | rejected | misses exact legal/document facts |
| diffusion as core LLM/VLM | delayed/research-only | not stable/open/product-suitable enough |
| training full VLM from scratch immediately | rejected now | requires data/evals first |
| internal MoE/5 agents inside model in v1 | rejected now | external evidence/harness first |
| court product as core repo scope | delayed/downstream | core must be general multilingual document intelligence |
| zero-native as main LibreOffice UI path | rejected now | poor fit for native LibreOffice |

## 3. Research-only lanes

These are interesting but must not block v1:
- hierarchical attention reference
- token-superposition-style pretraining reference
- neuron-preserving optimizer optimizer
- four-bit pretraining reference
- tile-wise sparse FFN
- world-model reference/world-model reference world models
- dLLM/diffusion text
- Natural Language Autoencoders
- Goodfire-style neural geometry probes

## 4. Near-term experiment lanes

Allowed after schema/API/eval skeleton exists:
- open-weight base-family candidate/open-weight base-family candidate base evaluation
- PEFT/LoRA/QLoRA/DoRA/LoRA+/adaptive low-rank training reference adapters
- extreme quantization reference/llama.cpp KV cache compression
- MLX/GGUF/ONNX runtime export
- WebGPU FilterTube tiny model prototype
- external document-layout reference-style visual layout UI
- CURIO-style dewarp/rectification

## 5. Final module naming decision

Use aKriti-owned names:

```text
aKriti Layout Reader
aKriti Text Reader
aKriti Table Reader
aKriti Chart Reader
aKriti Image Reader
aKriti Restoration Module
aKriti Translation Module
Kriti Reasoning/Action Module
aKriti Runtime
```

Do not describe final architecture as using external specialist engines.

## 6. Model tier decision

```text
aKriti Tiny  -> routing/embeddings/thumbnail/page triage
aKriti Small -> local OCR assist and simple document understanding
aKriti Core  -> around 3B main local document VLM/reasoning
aKriti Pro   -> 8B+ teacher/verifier/cloud/workstation
Kriti        -> reasoning/action/document-command module
```

## 7. Training ownership decision

Stages:

```text
1. harness and schema
2. open-weight adapters
3. distilled owned checkpoints
4. independent owned modules
5. local runtime packaging
```

Only after stage 3 should public language say "our model family" without caveat.

## 8. Safety and evidence decision

For legal/court and user documents:

```text
original remains evidence
restored output is derived artifact
generated summary is interpretation
native edit requires approval
all high-impact claims need provenance
```

## 9. Next model watch decision

Only one near-term external model event is allowed to affect base selection:

```text
open-weight base-family candidate open-weight release and model card.
```

All other new papers must enter the research ledger, not derail the build path.

### D014: External systems are inspiration-only; open weights are the only importable model artifact

**Decision:** aKriti will not depend on external OCR, VLM, PDF, translation, layout, video, restoration, or reasoning systems as product components, teacher systems, verifier systems, or label generators. External projects and papers may be studied, but the implementation must be aKriti-owned. The only external model artifact allowed into aKriti lineage is open weights, with license/provenance recorded in the model manifest.

**Rationale:** The project goal is an owned local-first document intelligence model family that can be integrated natively into LibreOffice, FilterTube, and later Vinti. Wrapping existing systems would not produce the ownership, portability, privacy, or low-compute control that the project needs.

## Research References

This doc is connected to the numbered research bibliography in `docs/akriti-research-reference-index.md`. Those references are engineering anchors for aKriti-owned implementation; they are not product dependencies. Only open weights may enter model lineage, and only with manifest provenance.

### D016: Periodic low-rank merge training is more relevant to aKriti Core than restoration personalization

**Decision:** Track periodic low-rank merge training as a later `aKriti Core` training prototype [24]. Track continual low-rank visual personalization as a later degraded-document restoration prototype [25]. Neither is a v1 blocker.

**Rationale:** Periodic low-rank merge training directly targets memory-efficient continued pretraining and domain adaptation for the Core model. Continual visual personalization is useful later for restoration domains such as old scans, stamps, seals, handwriting styles, blur, compression artifacts, and degraded Indic glyphs.

**Current priority:** Keep Phase 0/1 focused on contracts, fixtures, OCR/layout/table/chart interfaces, confidence/voting, and local runtime packaging. Do not add large-scale inference network topology work to the current roadmap.

### D017: aKritiExtract and aKritiMath are first-class Phase 1.5 capabilities

**Decision:** Add a Phase 1.5 layer for schema-guided extraction and Math/LaTeX document intelligence [26], [27]. This layer sits after initial OCR/layout/table/chart parsing and before full Kriti action/reasoning.

**Rationale:** LibreOffice users need structured document work, not only chat. The useful product shape is: select a page/region/document, define or generate an extraction schema, return typed values with evidence, and optionally write those values back into Writer, Calc, Impress, JSON, Markdown, or formula objects.

**Implementation boundary:** aKriti implements this in-house. External systems are references only. Open weights may be evaluated only through the manifest policy.

**Core field distinction:** `verbatim` means exact source-preserving text. `normalized` means interpreted or transformed content. Legal, financial, mathematical, and citation-bearing fields must expose this distinction.

### D018: aKriti must ground evidence regions and test precision stability before Vinti automation

**Decision:** Add `aKriti Grounding Module` and `aKriti Precision Harness` as required engineering lanes [30], [31], [32].

**Rationale:** Vinti cannot trust a text-only extraction. Court-file logistics requires locating the page regions behind claims: seals, signatures, case numbers, party names, paragraphs, tables, formulas, handwritten notes, and low-confidence spans. Runtime precision issues can also create malformed JSON, invalid bboxes, loops, or triage drift even when the model appears fluent.

**Implementation boundary:** External OCR/layout/grounding systems remain references, benchmark opponents, or temporary research controls only. Their weights are not product dependencies unless a separate manifest proves that a specific open-weight base is license-compatible and intentionally adopted.

**Product rule:** aKriti may propose a grounded region. Vinti must preserve uncertainty. Human review is required when grounding, OCR, restoration, or runtime precision is unstable.

### D019: Capability contracts come before downstream products

**Decision:** Translation, charts, restoration, actions, exports, grounding, and language metadata are first-class aKriti capability families [33], [34], [35], [36]. Their contracts, schemas, Workbench filters, provenance rules, review states, and minimal paths must exist before Vinti or LibreOffice depend on them.

**Rationale:** aKriti is a source-grounded document state platform, not an OCR text extractor. Vinti should consume aKritiDoc. LibreOffice should consume aKritiDoc/actions/exports. FilterTube should consume Tiny/local subsets. None of these downstream products should invent separate parsing or confidence systems.

**Implementation rule:** Build the substrate early and deepen quality over time:

```text
contract early -> minimal path early -> eval gate -> stronger model -> product integration
```

### D020: Language support is layered and must be measured

**Decision:** Adopt language support levels L0-L6 [35].

```text
L0 script detection
L1 OCR/text reading
L2 layout-aware reading order
L3 entity/terminology preservation
L4 translation/transliteration
L5 structured extraction/reasoning
L6 Vinti-grade legal/court reliability
```

**Rationale:** Saying "supports many languages" is technically useless unless support is separated by capability, evidence, and product risk. A language may be good enough for OCR review but not safe for court triage.

**Vinti safety rule:** No case triage claim can be high confidence if the supporting page/block/span has unresolved language, script, glyph, region, restoration, or precision ambiguity.

### D021: Voice is a separate Shruti lane, not aKriti core scope

**Decision:** aKriti input modalities are text prompts, images/pages, files/document bundles, regions/selections, and structured schemas. Voice/audio should become a separate `Shruti` lane only if it is useful enough to own and evaluate.

**Rationale:** Voice can later provide ASR/TTS/voice-command artifacts that call aKriti APIs, but adding voice into the core aKriti scope now would dilute the document VLM roadmap.

### D022: External research is clean-room input; commodity plumbing is allowed

**Decision:** aKriti is independently implemented. External OCR/VLM/document systems are references, baselines, temporary research controls, or open-weight candidates only. Commodity plumbing is allowed when it is replaceable and license-clean.

**Rationale:** Strategic ownership is in aKritiDoc, aKritiParse, OCR/VLM/layout/grounding/ambiguity/eval/voting/Workbench/Vinti workflow/ledger-feedback layers. Rewriting every image codec, PDF rasterizer, database driver, HTTP framework, or crypto primitive would slow the product without increasing strategic ownership.

**Rule:** Extract concepts, not code. Copying code requires an explicit license decision and intentional vendoring.

### D023: Feedback improves aKriti/Vinti offline, not by production self-mutation

**Decision:** Add an offline feedback/harness/adaptor improvement lane [37].

**Rationale:** Vinti has a natural analyzer harness: voters, thresholds, retry logic, evidence search, restoration/re-read paths, human review triggers, and ledger state. Human-reviewed corrections should improve both this harness and aKriti adapters, but live court workflows must remain versioned, auditable, evaluated, approved, and rollbackable.

**Blocked:** production self-training, silent prompt changes, silent threshold changes, unapproved analyzer weighting changes, and ledger events without model/harness/schema/analyzer versions.

### D024: Vinti Analyzer Pack is domain-specific; generic analyzers belong to aKriti

**Decision:** Use `aKriti Analyzer` for reusable document-intelligence analyzers, `Kriti Orchestrator` for generic local tool/action coordination, and `Vinti Analyzer Pack` for court-logistics analyzers [38].

**Rationale:** Vinti should not own or duplicate OCR, layout, grounding, translation, restoration, precision checks, or document parsing. Vinti is the court-logistics product layer that consumes aKritiDoc and aKriti analyzer outputs.

**Terminology rule:** `Vinti analyzer` is acceptable only as shorthand for a domain-specific court-logistics analyzer. Prefer `Vinti Analyzer Pack` in docs.

### D025: Device-agent lessons apply to orchestration, tokenizer benchmarks, and packaging

**Decision:** Track device-optimized active-parameter agent models as a runtime/orchestrator reference [38].

**Adopt:** local tool-calling architecture, active-parameter efficiency as future research, tokenizer efficiency metrics for Indic/legal text, abstention/loop-control evals, and multi-format runtime packaging.

**Reject for now:** text-only local agents as OCR/VLM core, training MoE first, raw chain-of-thought exposure, and whole-case context dumping instead of aKritiDoc retrieval/evidence pointers.

### D026: Encoder-free multimodal projection is a prototype lane, not a base-model lock

**Decision:** Track encoder-free multimodal projection as a prototype architecture lane [40].

**Rationale:** Projecting image patches and audio frames directly into a shared token space can reduce latency, memory, and adapter complexity. For aKriti, the idea is attractive for local-first Pro/Core models and multimodal action loops, but document OCR/layout quality must be proven on aKritiDoc evals before adoption.

**Rule:** Do not replace specialized document OCR/layout/grounding heads with encoder-free projection unless held-out document fixtures prove no regression on small text, Indic glyphs, dense tables, stamps, handwriting, degraded scans, bboxes, and reading order.

### D027: Shruti is the audio companion lane for LibreOffice/aKriti

**Decision:** Add `Shruti` as a separate audio lane that can provide ASR, TTS, speech translation, and voice-command artifacts [40].

**Rationale:** Audio-to-text, text-to-audio, and voice commands are useful for LibreOffice accessibility, dictation, read-aloud, and natural document actions. But audio should not dilute aKriti’s core scope as a source-grounded document state engine.

**Integration rule:** Shruti artifacts can call aKriti APIs and attach to aKritiDoc/actions. Voice commands must produce preview-first action requests; destructive edits require explicit confirmation.
