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
| `akriti-model-package-manifests.md` | [10]-[15], [20]-[22] | Manifests track open-weight lineage, quantization, tokenizer, adapter, and runtime choices. |
| `akriti-training-and-distillation-plan.md` | [1]-[8], [10]-[15], [19]-[22] | Training phases, internal mentor models, PEFT, quantization, tokenizer, reasoning, restoration. |
| `akriti-data-engine-and-synthetic-documents.md` | [8], [16], [19]-[22] | Synthetic documents, multilingual data, restoration tasks, translation tasks, structured labels generated by aKriti-owned tooling. |
| `akriti-research-ledger.md` | [1]-[23] | Canonical engineering watchlist and adoption/prototype/watch/reject decisions. |
| `akriti-research-to-experiment-matrix.md` | [1]-[23] | Converts engineering references into experiments, fixtures, gates, and ablations. |
| `akriti-fixture-corpus-and-experiment-cards.md` | [1], [8]-[9], [16], [19]-[22] | Fixture design for documents, low confidence, exact retrieval, multilingual, and restoration. |
| `akriti-baseline-bakeoff-protocol.md` | [9], [16], [19]-[22] | Benchmark opponent design and eval structure without depending on external systems. |
| `akriti-implementation-plan.md` | [9]-[15], [20]-[22] | Build order for runtime, quantization, adapters, exact search, and local packaging. |
| `akriti-hardware-experiment-plan.md` | [10]-[15], [17]-[18] | RTX 2060, Mac, CPU/RAM, WebGPU, cloud training, and edge runtime experiments. |
| `akriti-scaffold-implementation-backlog.md` | [9]-[15], [16], [19]-[22] | Engineering backlog for contracts, runtime, OCR/layout, translation, and restoration. |
| `akriti-repo-scaffold-blueprint.md` | [9], [10]-[15] | Repository layout supports evals, manifests, runtime targets, and experiment logging. |
| `akriti-glossary-and-naming.md` | [1]-[23] | Keeps naming consistent: engineering references are not dependencies; open weights are manifest-governed. |
| `akriti-decision-log.md` | [1]-[23] | Locked decisions and watch items cite numbered engineering anchors. |
| `akriti-next-agent-operating-guide.md` | [1]-[23] | Future agents route new papers into this engineering graph before changing architecture. |
| `akriti-research-docs-session-handoff-2026-05-20.md` | [1]-[23] | Session handoff and continuity map. |
| `akriti-docs-phase-completion-audit.md` | [1]-[23] | Audits whether docs cover the relevant engineering clusters. |

## Development-Cycle Mapping

| Cycle | Engineering anchors | What aKriti implements in-house |
|---|---:|---|
| Phase 0: contracts and corpus | [9], [16], [19], [20]-[22] | `aKritiDoc`, fixtures, evidence regions, multilingual spans, restoration provenance, exact-search-first retrieval. |
| Phase 1: OCR/layout/table/chart | [16], [19], [20] | Owned page encoder, text reader, layout reader, table reader, chart reader, confidence spans. |
| Phase 2: reasoning and voting | [1]-[8], [23] | Kriti candidate generation, multi-pass voting, abstention, verifier traces, low-confidence UX. |
| Phase 3: efficient training | [10]-[15], [20] | Efficient pretraining trials, adapter experiments, quantization-aware export, tokenizer ablations. |
| Phase 4: restoration and translation | [19], [21], [22] | Non-destructive visual restoration, Indic translation, bilingual document preservation, uncertainty reporting. |
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
