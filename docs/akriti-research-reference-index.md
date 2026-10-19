# aKriti Engineering Reference Index

Date: 2026-05-20
Status: Repo-facing engineering bibliography for `docs/akriti-*.md`

## Boundary

This repo does not treat external projects, products, repos, or systems as product dependencies. It also avoids naming them in project-facing architecture docs.

References below are numbered engineering anchors. They describe the kind of implementation work aKriti will do in-house. The only external artifact allowed into aKriti model lineage is open weights, and only with license, version, role, and provenance recorded in the model manifest.

Named paper/project notes are kept outside the repo in private local research notes.

## Citation Style

Use numbered references like `[1]`, `[2]`, `[3]` in architecture, training, runtime, and evaluation docs.

Example:

`Kriti's ambiguity/voting loop is guided by stochastic and recursive reasoning references [1]-[5], but the implementation is aKriti-owned.`

## Numbered Engineering References

### Recursive reasoning, voting, confidence, and Kriti

[1] Stochastic recursive reasoning — multi-trajectory latent reasoning, alternative solution paths, parallel candidate sampling, confidence through candidate agreement/disagreement. Source: <https://arxiv.org/abs/2605.19376>

[2] Hierarchical recurrent reasoning — slow/abstract planning plus fast/detail computation, compact recurrent reasoning, small-data structured task learning. Source: <https://arxiv.org/abs/2506.21734>

[3] Tiny iterative refinement — small networks gaining effective depth through repeated refinement; useful for local reasoning loops and ablation design. Source: <https://arxiv.org/abs/2510.04871>

[4] Fixed-point attractor refinement — adaptive convergence, implicit-depth computation, and constant-memory iterative reasoning. Source: <https://arxiv.org/abs/2605.12466>

[5] Autoregressive recursive refinement caution — controlled experiments showing where recursive refinement helps and where it does not transfer cleanly. Source: <https://arxiv.org/abs/2603.08082>

[6] Structured latent deduction — projection through constrained latent spaces, soundness checks, and abstention when a solution cannot be trusted. Source: <https://arxiv.org/abs/2605.08605>

[7] Lattice-style reduction math — iterative basis reduction concepts for structured deduction and constrained search experiments. Source: <https://arxiv.org/abs/2303.02226>

[8] Hard-reasoning curriculum and verifiable rewards — reverse-difficulty curricula, self-checking, verifiable reward training, proof-level review, and test-time scaling. Source: <https://arxiv.org/abs/2605.13301>

[9] Exact-search-first agent retrieval — exact text search, file/tool-output presentation, and retrieval harness design for document workflows. Source: <https://arxiv.org/abs/2605.15184>

### Pretraining efficiency and long context

[10] Token-bag early pretraining — faster early-stage pretraining with contiguous token groups, then recovery to standard next-token training. Source: <https://arxiv.org/abs/2605.06546>

[11] Training-only hierarchical attention — long-context sequence selection during training with recovery to ordinary dense attention for deployment. Source: <https://arxiv.org/abs/2605.06554>

[12] GPU-shaped sparse feedforward execution — activation sparsity, GPU-friendly sparse packing, and custom kernels for sparse transformer efficiency. Source: <https://arxiv.org/abs/2603.23198>

[13] Four-bit pretraining stability — random rotations, stochastic rounding, two-dimensional quantization, and selective high-precision layers. Source: <https://arxiv.org/abs/2509.25149>

### Fine-tuning, adapters, and compression

[14] Adaptive low-rank adapter discovery — dynamic rank allocation, parallel component learning, and self-correcting low-rank updates. Source: <https://arxiv.org/abs/2605.10741>

[15] Rotation-based vector quantization — online vector quantization, KV-cache compression, embedding compression, and distortion-aware retrieval. Source: <https://arxiv.org/abs/2504.19874>

[24] Periodic low-rank merge training — repeated low-rank update, merge, reset cycles for memory-efficient continued pretraining and model adaptation. Source: <https://arxiv.org/abs/2307.05695>

[25] Continual low-rank visual personalization — low-rank continual adaptation for visual restoration domains while reducing forgetting across document degradation styles. Source: <https://arxiv.org/abs/2410.04891>

### Vision-language perception and document understanding

[16] Text-conditioned visual encoding — query-aware image/page encoding, tile reduction, and region selection for document understanding. Source: <https://ai.meta.com/research/publications/text-guided-semantic-image-encoder/>

[17] Compact pixel-to-latent world modeling — small visual predictive models, latent regularization, surprise scoring, and physical-plausibility checks. Source: <https://le-wm.github.io/>

[18] Video world-model benchmarking — visual prediction, physical reasoning benchmarks, and planning concepts for future video/document interaction. Source: <https://ai.meta.com/blog/v-jepa-2-world-model-benchmarks/>

[19] Process-driven visual restoration — plan/sketch/inspect/refine loops for glyph, stamp, seal, degraded text, and non-destructive document restoration. Source: <https://arxiv.org/abs/2604.04746>

### Tokenization, multilingual, and Indic-language strategy

[20] Compute-aware tokenization — token granularity, byte-level scaling, and multilingual tokenizer selection for local models. Source: <https://ai.meta.com/research/publications/compute-optimal-tokenization/>

[21] Massive multilingual translation specialization — decoder-only and encoder-decoder translation specialization patterns for multilingual and Indic coverage. Source: <https://arxiv.org/abs/2603.16309>

[22] Cross-lingual/cross-modal embeddings — multilingual semantic alignment across text, speech, code, math, and document retrieval. Source: <https://arxiv.org/abs/2603.16606>

### Interpretability, verification, and safety

[23] Activation-to-language interpretability — converting latent activations into human-readable descriptions for audit, debugging, and verifier design. Source: <https://transformer-circuits.pub/2026/nla/index.html>

## aKriti Doc-to-Reference Map

| Doc | Primary engineering anchors | Implementation meaning |
|---|---:|---|
| `akriti-master-architecture.md` | [1]-[5], [10]-[18], [20]-[22] | Overall model family, perception, reasoning, runtime, multilingual direction. |
| `akriti-system-diagrams-and-feature-map.md` | [1], [9], [16]-[22] | Feature blocks: OCR/layout, charts, translation, local semantic filtering, confidence/voting. |
| `akriti-api-capability-map.md` | [1], [9], [16], [19], [21], [22] | API outputs expose evidence, confidence, exact search, translation, restoration uncertainty. |
| `akriti-contract-schema-implementation-spec.md` | [9], [16], [19], [21], [22] | Schema preserves exact evidence, regions, multilingual spans, restoration provenance, uncertainty. |
| `akriti-model-package-manifests.md` | [10]-[15], [20]-[22], [24], [25] | Manifests track open-weight lineage, quantization, tokenizer, adapter, periodic merge training, restoration adaptation, and runtime choices. |
| `akriti-training-and-distillation-plan.md` | [1]-[8], [10]-[15], [19]-[22], [24], [25] | Training phases, internal mentor models, PEFT, periodic low-rank merge training, quantization, tokenizer, reasoning, restoration. |
| `akriti-data-engine-and-synthetic-documents.md` | [8], [16], [19]-[22], [25] | Synthetic documents, multilingual data, restoration tasks, translation tasks, structured labels generated by aKriti-owned tooling. |
| `akriti-research-ledger.md` | [1]-[25] | Canonical engineering watchlist and adoption/prototype/watch/reject decisions. |
| `akriti-research-to-experiment-matrix.md` | [1]-[25] | Converts engineering references into experiments, fixtures, gates, and ablations. |
| `akriti-fixture-corpus-and-experiment-cards.md` | [1], [8]-[9], [16], [19]-[22] | Fixture design for documents, low confidence, exact retrieval, multilingual, and restoration. |
| `akriti-baseline-bakeoff-protocol.md` | [9], [16], [19]-[22] | Benchmark opponent design and eval structure without depending on external systems. |
| `akriti-implementation-plan.md` | [9]-[15], [20]-[22], [24], [25] | Build order for runtime, quantization, adapters, exact search, restoration adaptation, and local packaging. |
| `akriti-hardware-experiment-plan.md` | [10]-[15], [17]-[18] | RTX 2060, Mac, CPU/RAM, WebGPU, cloud training, and edge runtime experiments. |
| `akriti-scaffold-implementation-backlog.md` | [9]-[15], [16], [19]-[22] | Engineering backlog for contracts, runtime, OCR/layout, translation, and restoration. |
| `akriti-repo-scaffold-blueprint.md` | [9], [10]-[15] | Repository layout supports evals, manifests, runtime targets, and experiment logging. |
| `akriti-glossary-and-naming.md` | [1]-[23] | Keeps naming consistent: engineering references are not dependencies; open weights are manifest-governed. |
| `akriti-decision-log.md` | [1]-[25] | Locked decisions and watch items cite numbered engineering anchors. |
| `akriti-next-agent-operating-guide.md` | [1]-[25] | Future agents route new papers into this engineering graph before changing architecture. |
| `akriti-research-docs-session-handoff-2026-05-20.md` | [1]-[25] | Session handoff and continuity map. |
| `akriti-docs-phase-completion-audit.md` | [1]-[25] | Audits whether docs cover the relevant engineering clusters. |

## Development-Cycle Mapping

| Cycle | Engineering anchors | What aKriti implements in-house |
|---|---:|---|
| Phase 0: contracts and corpus | [9], [16], [19], [20]-[22] | `aKritiDoc`, fixtures, evidence regions, multilingual spans, restoration provenance, exact-search-first retrieval. |
| Phase 1: OCR/layout/table/chart | [16], [19], [20] | Owned page encoder, text reader, layout reader, table reader, chart reader, confidence spans. |
| Phase 2: reasoning and voting | [1]-[8], [23] | Kriti candidate generation, multi-pass voting, abstention, verifier traces, low-confidence UX. |
| Phase 3: efficient training | [10]-[15], [20], [24] | Efficient pretraining trials, adapter experiments, periodic low-rank merge training, quantization-aware export, tokenizer ablations. |
| Phase 4: restoration and translation | [19], [21], [22], [25] | Non-destructive visual restoration, continual restoration-domain adaptation, Indic translation, bilingual document preservation, uncertainty reporting. |
| Phase 5: runtimes and integrations | [9]-[15], [16] | LibreOffice native service, local CPU/GPU runtimes, browser/WebGPU module, model package manifests. |
| Phase 6: downstream products | [1], [9], [16]-[22] | Local semantic filtering, legal/court-document workflows later, product APIs built around aKriti-owned modules. |

## Immediate Interpretation for aKriti Voting

The strongest new direction is stochastic recursive reasoning [1]. For aKriti this becomes:

1. Generate multiple candidate interpretations when OCR/layout/chart/translation is ambiguous.
2. Score candidates against page evidence, exact citations, layout constraints, language constraints, and user intent.
3. Expose low confidence when candidates disagree materially.
4. Ask before destructive document edits.
5. Scale candidate width by hardware: small width on CPU/WebGPU, larger width on workstation/cloud.

This is an aKriti-owned engineering design, not a dependency on any external project.

## Added Engineering References: Schema Extraction and Math

[26] Schema-guided document extraction — typed extraction schemas, natural-language schema generation, verbatim-vs-normalized values, valid JSON contracts, and tree-structured extraction scoring. Source details kept in private research notes.

[27] Math and LaTeX document intelligence — rendered/scanned formula detection, LaTeX generation, MathML conversion, LibreOffice formula-object conversion, symbol-level confidence, and formula evidence grounding. Source details kept in private research notes and product requirements.

## Phase 1.5: aKritiExtract and aKritiMath

Add a dedicated phase between OCR/layout and full Kriti reasoning:

```text
Phase 1.5: Schema-guided extraction + Math/LaTeX lane
```

What aKriti implements in-house:

- `aKritiExtract`: schema-guided structured extraction from page, selection, region, or whole document.
- `aKritiMath`: formula detection, OCR-to-LaTeX, LaTeX-to-formula, MathML conversion, and LibreOffice formula insertion.
- Typed extraction fields with evidence and confidence.
- Natural-language-to-schema generation.
- Fast mode for simple extraction and Kriti voting mode for ambiguous/high-risk fields.
- Export paths to JSON, Markdown, HTML, ODT fields, Calc tables, and LibreOffice Math objects.

This phase uses [26] and [27] as engineering anchors and remains fully aKriti-owned.

## Added Public-Sector Engineering Context

[28] NBF-compatible permissioned ledger architecture — court/government-controlled validator nodes, off-chain payloads, on-chain hashes, open API integration, append-only workflow states. Source: <https://blockchain.meity.gov.in/>

[29] Court logistics and arrears-reduction workflow requirements — prioritization, case distribution, unready/stayed case handling, ADR/ODR, monitoring, technology use, and systemic bottleneck identification. Source: <https://www.pib.gov.in/PressReleasePage.aspx?PRID=2205769&reg=3&lang=2>

Vinti maps [28] and [29] into an owned implementation: aKriti reads the case file; Vinti analyzer loops classify and route the file; court-controlled ledger adapters record hashes and workflow states.

## Added Engineering References: Document Parsing, Grounding, and Precision Stability

[30] Structured OCR-layout-table output schema — single-pass or coordinated document VLM output with layout labels, reading order, HTML/table/math representation, polygons, bboxes, confidence, skipped/error states, and page image bounds. Source details kept in private research notes because repo-facing docs should preserve the aKriti-owned implementation boundary.

[31] Parallel visual grounding and query-to-region localization — natural-language region localization, atomic bbox prediction, hybrid fast/slow decoding, OCR localization, layout grounding, GUI/document grounding, and dense-region evidence selection. Source details kept in private research notes because product docs should not imply dependency on external grounding weights.

[32] Precision-stability testing for linear-attention and low-precision inference — numerical-stability risk in long-context/linear-attention variants, low-precision floating-point behavior, loop/malformed-output detection, and safe fallback policy for legal/document inference. Source: <https://arxiv.org/abs/2605.21325>

These anchors add one owned module and one owned safety gate:

- `aKriti Grounding Module`: query-to-region localization for evidence, stamps, signatures, paragraphs, tables, formulas, handwritten notes, and low-confidence regions.
- `aKriti Precision Harness`: FP32/FP16/BF16/quantized regression tests for OCR text, JSON validity, bbox stability, reading order stability, loop rate, and Vinti triage consistency.

## Added Engineering References: Layered Workbench, Language Levels, Compact Multilingual VLMs, and Edge Runtime

[33] Layered document-intelligence workbench — visual grounding, semantic layout, block-level extraction, human correction, filterable layers, proofing, and source-region review. Source details kept in private research notes.

[34] Compact multilingual document VLM training — small/medium VLMs can perform strong OCR/layout/table/chart/document parsing when paired with targeted data curation, dynamic visual resolution, semantic layout supervision, reading-order objectives, continual pretraining, SFT, and verifiable-reward training. Source details kept in private research notes.

[35] Formal language-support levels — script detection, OCR, layout-aware reading order, entity/terminology preservation, translation/transliteration, structured extraction/reasoning, and high-stakes domain reliability must be measured separately. Source details kept in private research notes and aKriti product requirements.

[36] Edge/on-device runtime and Rust-host packaging — PyTorch/JAX research models should export to replaceable runtime packages for desktop, mobile, browser, and NPU/GPU acceleration; Rust or C++ host layers can provide safe local services and bindings without changing training ownership. Source: <https://developers.googleblog.com/en/litert-the-universal-framework-for-on-device-ai/>

These anchors change the roadmap ordering:

```text
Capability contracts early.
Minimal deterministic/stub paths early.
High-quality model depth staged.
Product-specific integration later.
```

Translation, charts, restoration, actions, and exports are not late add-ons. Their schemas, UI filters, provenance rules, review states, and minimal paths must exist before downstream products depend on aKritiDoc.

## Added Engineering Reference: Offline Feedback Loops and Harness Updates

[37] Self-improving analyzer harness and adapter-update loop — joint improvement through harness changes, prompts/tools/retry/search procedure, feedback-agent proposals, and model-weight/adaptor updates, gated by offline evaluation and versioned release. Source: <https://arxiv.org/abs/2605.27276>

For aKriti/Vinti this is not an OCR core reference. It is an improvement-system reference:

```text
human-reviewed correction
    -> failure log
    -> feedback proposal
    -> harness update candidate and/or adapter training candidate
    -> offline eval
    -> human approval
    -> versioned release
    -> rollbackable deployment
```

Production rule:

```text
No production self-training.
No silent prompt/threshold updates.
No unversioned analyzer changes.
No ledger event without model, schema, harness, and analyzer versions.
```

## Added Engineering Reference: Device-Optimized Agent Models and Active-Parameter Efficiency

[38] Device-optimized active-parameter agent models — local-first tool calling, hybrid active-parameter efficiency, long-context local agents, expanded multilingual tokenizers, multi-format runtime packaging, abstention tuning, loop reduction, and on-device/private workflow orchestration. Source details kept in private research notes.

For aKriti/Vinti this is not an OCR/VLM-core replacement. It is a runtime, tokenizer, and orchestrator reference:

```text
aKriti VLM/readers produce aKritiDoc.
Kriti Orchestrator coordinates tools/actions over aKritiDoc.
Vinti Analyzer Pack applies court-logistics questions over aKritiDoc.
```

New benchmark family:

```text
aKritiTokenizerBench
  chars/token per language/script
  tokens/page
  tokens/case bundle
  citation/section tokenization
  Indic glyph ambiguity tokenization
```

New runtime/product rule:

```text
Do not expose raw chain-of-thought.
Expose structured evidence, analyzer votes, tool calls, review reasons, and page-grounded rationales.
```

## Added Engineering Reference: Deterministic Grid Projection for PDF Text Layers

[39] Deterministic grid projection for born-digital PDF text layers — coordinate-to-line grouping, recurring alignment anchors, left/right/center/floating snap classes, paragraph escape hatch, monospace grid projection, forward anchors, sparse-block compression, margin cleanup, and visual/debug traces. Source details kept in private research notes.

For aKriti this is an `aKritiParse` technique:

```text
PDF text fragments with coordinates
    -> line grouping
    -> anchor extraction
    -> snap classification
    -> flowing-text bypass
    -> grid projection
    -> post-processing
    -> aKritiDoc blocks/spans with debug provenance
```

It is useful for:

- born-digital PDFs.
- text-layer PDFs with tables/schedules.
- financial statements and forms.
- court orders with headers, stamps, footnotes, paragraphs, and aligned metadata.
- cheap deterministic fast path before invoking OCR/VLM.

It is not enough for:

- scanned PDFs with no text layer.
- stamps/signatures as image artifacts.
- handwriting.
- degraded text.
- semantic case triage.

## Added Engineering Reference: Encoder-Free Multimodal Projection and Audio Bridge

[40] Encoder-free multimodal projection — image patches/audio frames projected directly into a shared LLM token space through lightweight projection layers, reducing separate encoder latency and simplifying one-pass adapter/full-model tuning across text, image, and audio. Source details kept in private research notes.

For aKriti this is a `prototype after baseline VLM evals` architecture lane:

```text
image/page/crop patches
audio frames via Shruti
text tokens
    -> lightweight projection into shared token space
    -> document/action model
    -> aKritiDoc or Shruti artifact output
```

Potential benefits:

- lower memory footprint.
- lower multimodal latency.
- simpler LoRA/adaptor path across modalities.
- local-first text/image/audio interaction.
- useful voice-command path for LibreOffice through `Shruti`.

Risks:

- fine-grained OCR and Indic glyph recognition may still need specialized visual encoders/heads.
- dense tables, stamps, handwriting, degraded scans, and small text require document-native evals before adoption.
- voice/audio should not expand aKriti core scope; it should enter through a separate `Shruti` lane.

## Added Engineering Reference: Cross-Layer Sparse-Attention Index Reuse

[41] Cross-layer sparse-attention index reuse — long-context sparse-attention inference optimization that reuses top-k token index selections across adjacent layers, reducing indexer computation while preserving sparse attention quality. Source: <https://arxiv.org/abs/2603.12201>

For aKriti this is a later `aKriti Pro` / `aKriti Vinti` runtime optimization reference:

```text
sparse-attention base model
    -> some full-indexer layers
    -> shared layers reuse nearest full layer indices
    -> lower prefill/decode cost
```

Use only if:

- a future aKriti Pro/Vinti base actually uses sparse attention with an explicit indexer.
- indexer cost is measurable in long-document inference.
- quality on aKritiDoc/Vinti evals remains stable.

Do not use this to justify dumping entire case files into context. Prefer aKritiDoc indexing, exact search, page/block retrieval, case graphs, and evidence pointers first.

## Added Engineering Reference: Open-Weight Multimodal Model Family for Core/Pro Benchmarking

[46] Open-weight multimodal model family with encoder-free projection, quantization-aware packaging, speculative/multi-token decoding, document benchmark relevance, and mixed-size local/server deployment tiers. Source details kept in private research notes because public docs should preserve benchmark-driven model selection rather than model-name loyalty.

Interpretation for aKriti:

- Treat the family as a benchmark/prototype candidate for `aKriti Small`, `aKriti Core`, `aKriti Pro`, and `Kriti` orchestration.
- Evaluate smaller variants for local LibreOffice assist and larger variants for Pro/teacher/verifier lanes.
- Do not replace existing open-weight candidate pools until aKritiDoc fixtures prove better OCR/layout/table/chart/grounding/runtime behavior.
- Use encoder-free multimodal projection, QAT/runtime packaging, tokenizer design, and speculative decoding as engineering ideas that must survive aKriti-owned evals.

## Added Engineering References: Efficient Multimodal Candidate Classes

[47] Native-resolution sparse multimodal reasoning — adaptive/native-resolution vision, sparse active language parameters, and long-context document understanding. Use as an architecture hypothesis and optional open-weight candidate only after license and owned fixture gates. Source details kept in private research notes.

[48] Very-large sparse native multimodality — unified image/audio/text reasoning, hybrid local/global attention, calibrated uncertainty, variable compute, and tool-assisted visual inspection. Reject as a local model; retain only independently implementable hypotheses. Source details kept in private research notes.

[49] VLM leaderboard and survey catalogs — useful for candidate discovery and benchmark taxonomy, but rankings are not promotion evidence. Every discovered candidate must be traced to its primary paper/model card and then evaluated on aKriti-owned fixtures. Source details kept in private research notes.

## 2026-10-14 Added Engineering References: Compact Reading and Local Inference

[50] Compact CPU sequence recognition inside a modular document pipeline, with native-text routing, layout geometry and explicit limitations. Source: <https://github.com/datalab-to/indra>. Named benchmark and model-license assessment is private; no engine or weights are adopted.

[51] Extreme binary latent factorization: effective storage relative to original matrices, learned scales, recovery training and residual capacity. Source: <https://arxiv.org/html/2506.13771v5>.

[52] Latent geometry alignment before binarization. Source: <https://proceedings.mlr.press/v306/lee26y.html>.

[53] Sensitivity-aware variable-rank binary acoustic compression. Source: <https://www.isca-archive.org/interspeech_2026/yeo26_interspeech.html>.

[54] Acoustic-transition-aware low-bit calibration. Source: <https://www.isca-archive.org/interspeech_2026/jeon26c_interspeech.html>.

[55] Training-time frozen-layer streaming for resident adapters; not deployment compression or universal QAT. Source: <https://github.com/MakazhanAlpamys/Soup/blob/main/docs/performance-and-quantization.md>.

[56] Depth-conditioned recurrent acoustic encoders with intermediate CTC supervision and prediction feedback. Source: <https://arxiv.org/html/2606.04678v1>.

[57] Restricted latent recursion with unique boundary layers; encoder savings and fixed-loop accuracy tradeoffs. Source: <https://arxiv.org/html/2606.09357v1>.

[58] Group-wise acoustic recursion respecting depth specialization. Source: <https://ojs.aaai.org/index.php/AAAI/article/view/40386>.

[59] Confidence-gated early exits in acoustic recognition. Source: <https://www.isca-archive.org/interspeech_2024/yoon24_interspeech.html>.

[60] Modality-aware extreme low-bit weight representation. Source: <https://arxiv.org/abs/2609.24875>.

[61] Coordinated quantization and visual-token allocation. Source: <https://arxiv.org/abs/2604.17320>.

Apply these through [the owned CPU/compression plan](akriti-cpu-text-reader-and-local-compression-plan.md). References are not software dependencies, label sources or product capability claims. Speech-only results do not establish document/VLM accuracy or bounded device execution.

## 2026-10-14 Visual Retrieval and Vector Storage References

[62] Multimodal page-token embeddings and late-interaction scoring contract. Sources: <https://huggingface.co/perplexity-ai/pplx-embed-v2-late-9b> and <https://huggingface.co/perplexity-ai/pplx-embed-v2-late-0.6b>.

[63] Compressed page-image retrieval with original-vector rescoring; scoped English evaluation and storage projections. Source: <https://qdrant.tech/blog/pplx-embed-v2/>.

[64] Near-optimal-distortion online vector quantization. Source: <https://arxiv.org/abs/2504.19874>.

[65] Calibrated rotated quantization, asymmetric query scoring and hybrid low-bit implementation details. Source: <https://qdrant.tech/articles/turboquant-quantization/>.

[66] Compressed/original-vector persistence, representation overhead and rescoring behavior. Source: <https://qdrant.tech/documentation/manage-data/quantization/>.

Apply these through [the owned visual retrieval engineering plan](akriti-visual-retrieval-engineering-plan.md). No external vector store, runtime, on-disk format or teacher pipeline is adopted. Open-weight approval remains separate. Retrieval similarity is not source verification; vector compression is not model-weight compression or a total-device memory result.

## 2026-10-18 Source Evidence, Selective Risk and Context References

[67] Unicode text segmentation, extended grapheme clusters and declared tailoring. Source: <https://www.unicode.org/reports/tr29/>. Pin the implementation's Unicode/profile version; this is not an OCR recognizer.

[68] Calibrated control of expected monotone loss. Source: <https://arxiv.org/abs/2208.02814>.

[69] Selective prediction with explicit accepted-case risk and calibration assumptions. Sources: <https://arxiv.org/abs/2603.24704> and <https://arxiv.org/abs/2512.12844>.

[70] Dynamic early-exit acoustic models and exit-selection training. Source: <https://arxiv.org/abs/2309.09546>. Does not establish document-model recursion or streaming-state compatibility.

[71] Context selection, over-biasing and difficult negative candidates. Sources: <https://www.isca-archive.org/interspeech_2023/harding23_interspeech.html> and <https://www.isca-archive.org/interspeech_2023/bleeker23_interspeech.html>.

[72] Acoustic candidate conditioning without a synthesis pipeline; compatible intermediate/self-conditioned CTC prerequisites and reference-derived keyword-inventory caveat. Source: <https://arxiv.org/html/2506.01263v1>.

[73] Separate spoken/written representations and selective formatting responsibilities. Sources: <https://arxiv.org/html/2609.02901v1> and <https://arxiv.org/abs/2501.05948>.

Apply through [the evidence/adaptive-compute plan](akriti-evidence-and-adaptive-compute-research-plan.md). Speech references motivate controlled cross-modality hypotheses, not document accuracy or approved implementations. No external code, labels, teacher outputs or verifier decisions are adopted; no statistical guarantee applies without its assumptions and actual calibration.

## 2026-10-19 Routed CPU Readers and Conditional Specialists

[74] Compact routed OCR, expanded scanned-script coverage, auxiliary language models and conditional recognition: source snapshot reviewed 2026-10-19. Sources: <https://github.com/datalab-to/indra> and <https://huggingface.co/datalab-to/indra-ocr>. Main-recognizer parameters, complete package, optional math, benchmark mode and CPU threads are separate measurements.

[75] Separately licensed OCR weight artifact, including use and derivative restrictions. Source: <https://huggingface.co/datalab-to/indra-ocr/blob/main/LICENSE>. Code-license permission does not approve weights, outputs or implementation imports under aKriti's ownership policy.

Apply through [the owned Indic CPU-reader supplement](akriti-indic-cpu-reader-and-selective-processing-plan.md), EXP-038 through EXP-041 and the existing CPU/reread controls. References are not dependencies, teachers, label sources or verifiers. Expanded vendor coverage does not establish our Indic accuracy or handwritten-document capability.
