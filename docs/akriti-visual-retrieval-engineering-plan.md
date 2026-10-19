# aKriti Owned Visual Retrieval Engineering Plan

**Date:** 2026-10-14  
**Status:** Planned engineering and research; no engine implementation or benchmark is delivered by this document.  
**Scope:** Local document discovery, source-grounded page/region access, and later bounded collection search.  
**References:** [62]-[66] in [the engineering reference index](akriti-research-reference-index.md).

## 1. Decision and product boundary

Build our own retrieval engine. Qdrant and FAISS are not required dependencies, runtime services, deployment requirements or interchange formats. External implementations are research references, not code, generated-label or teacher-pipeline imports. License-compatible open weights remain a separately approved option, with source/version/license and role recorded in the model manifest.

aKriti remains a general-purpose document VLM platform. Retrieval selects evidence to inspect; the owned reader/VLM and verification path establish what the source says. Kriti actions, LibreOffice document writes and Vinti case workflows stay outside the ranking engine. Retrieval scores do not authorize edits or legal workflow changes.

Start with one open document on one machine. Do not make a large corpus, a server, a 9B embedding model or extreme-bit kernels a prerequisite for the first useful product.

## 2. What this adds, and what it does not

| Capability | Intended role | Boundary |
|---|---|---|
| Exact text lookup | Find names, identifiers, dates, amounts and citations | A broken or absent text layer still needs source reading |
| Layout/table/chart lookup | Retrieve existing aKritiDoc evidence units | Depends on trustworthy extracted structure |
| Page-image embeddings | Discover pages by visual/textual content without trusting OCR first | Not transcription or exact-field verification |
| Late interaction | Match different query tokens to different page token vectors | Relevance score, not truth or calibrated confidence |
| Compressed scoring | Reduce stored-vector traffic and candidate-ranking cost | Does not compress model weights or all runtime memory |
| Original-vector rescoring | Improve ordering among retained candidates | Cannot recover an excluded page |
| Evidence resolution | Open original page/region and run the owned reading/verification path | Required before source-backed extraction or action |

Use image retrieval for visually meaningful pages, weak OCR and missing text layers. Preserve exact lookup for literal queries. OCR-free discovery and accurate OCR/extraction are different capabilities; both can coexist.

## 3. End-to-end flow

```text
INGEST
original bytes -> document/page identity -> stable rendered pages
                       |
                       +-> trustworthy extraction -> exact/layout index
                       |
                       +-> page encoder -> token vectors -> versioned local store

QUERY
user request -> literal/semantic/visual intent + permitted source scope
                       |
                       +-> exact candidates
                       +-> visual candidates
                                |
                         candidate union, deduplication and ranking
                                |
                         optional compressed MaxSim
                                |
                         retained original-vector rescore
                                |
                         original page/region access
                                |
                         owned reader/VLM and verification
                                |
                         aKritiDoc evidence -> citation / review / abstention
```

An unparsed page may be visually indexed once its source and page identity exist. It need not wait for complete OCR. Its retrieved content must still be read and grounded before being used as extracted evidence.

## 4. Owned module contracts

These are proposed responsibilities, not claims that corresponding modules already exist.

| Interface | Responsibilities |
|---|---|
| Source catalog | Document/page identity, source hash, render transform, revisions, permitted scope and original-source resolution |
| Exact index | Literal and normalized lookup with separate modes, postings and links to evidence units |
| Page/query encoder interface | Input preparation, approved model manifest, space identifier, dimensions, masks and normalization |
| Multi-vector store | Contiguous vector blocks, page-offset/count table, dtype/codec metadata and bounds |
| Candidate generator | Scope first; bounded scan initially; later optional coarse index; candidate budget and deduplication |
| MaxSim scorer | Declared float reference, query/page token masking, deterministic tie-breaking and comparable scores within one contract |
| Vector codec | Encode/decode/score representations with explicit rotation, scales, codebooks, norms and version where applicable |
| Original-vector rescorer | Bounded candidate reads, original-vector scoring and disk-I/O accounting |
| Evidence resolver | Return source/page pointers; resolve to verified aKritiDoc blocks/cells/regions where available |

Use Python and array/tensor operations to understand the reference scorer first. Move measured bottlenecks to an owned C++/Rust/native backend later. The numerical contract must remain the same across backends; a faster implementation is not a reason to silently change ranking.

Persistence can start with simple local files and a small catalog. Using ordinary storage primitives is not the same as outsourcing the decision engine. There is no need to recreate a general-purpose distributed database before shipping single-document retrieval.

## 5. Late-interaction concept and cost

Let a query have token vectors `q_i` and a page have token vectors `p_j` in the same embedding space. For valid, unmasked tokens, the reference score is:

```text
score(query, page) = sum_i max_j dot(q_i, p_j)
```

If the model contract uses unit-normalized vectors, the dot product is cosine similarity. Do not silently normalize a contract that expects another scoring rule. Do not initialize maxima to zero: valid similarities may all be negative. Pages with no valid vectors require explicit handling, not a fabricated score.

One query token can match a heading while another matches a legend or table entry. This is the attraction of multiple page vectors compared with one pooled page vector.

The straightforward cost per page is proportional to `query_tokens * page_tokens * dimensions`. Work grows with corpus size. A compressed full scan is still a full scan; storage compression does not make it an approximate-nearest-neighbor index or prove million-page interactive search.

Token/patch positions are not automatically trustworthy text bounding boxes. Preserve the encoder's actual mapping, if provided, and resolve precise evidence through the original render and owned layout/reader path.

## 6. Minimal persistent index contract

Record at least:

| Field group | Required information |
|---|---|
| Source identity | Document ID, source revision/hash, page ID/number and permitted collection/case scope |
| Render identity | Renderer/preprocessing revision, size/resolution, rotation, crop and transform back to original coordinates |
| Embedding identity | Encoder revision, model-manifest reference, shared-space ID, dimensions, masks and normalization |
| Vector location | Page offsets/counts, byte order, dtype/codec version and block checksums |
| Codec state | Scales, norms, rotation seed/matrix, calibration version or codebook where used |
| Retrieval state | Index revision, deletions/tombstones, candidate policy and permission/filter revision |

Use bounded reads and validate offsets against file lengths. Reject incompatible, incomplete or corrupt blocks. Build into a new revision and publish it atomically, rather than exposing a partially rebuilt index.

An unchanged source/render/encoder contract can reuse derived vectors. A changed source or render invalidates affected pages. A changed embedding space requires re-encoding; changing only a codec may reuse retained floating-point vectors. Keep these operations distinct.

Cache keys must include source/index revision, embedding-space and encoder contract, codec and permitted scope. Permission revocation must invalidate scoped caches. Deleted documents lose derived vectors and search entries; retention/export rules remain explicit.

## 7. Compression and rescoring ladder

| Stage | Representation | Purpose |
|---|---|---|
| Control | Float32 with declared model scoring | Reference rankings and numerical behavior |
| First storage ablation | Float16 | Measure reduced bytes with limited implementation complexity |
| First quantized ablation | INT8 with recorded scales | Learn quantization error and asymmetric scoring |
| Later ablations | Selected 4/2-bit or rotated 1-bit representations | Investigate further bandwidth/storage reduction |
| Recovery ablation | Fixed candidate oversampling plus original-vector scoring | Measure ranking recovery and added I/O |

Change one factor at a time. Freeze page renders, encoder/query revisions, query labels and candidate policy when testing vector precision. Treat candidate oversampling and rescoring as explicit additional factors, not hidden changes that make a low-bit comparison look better.

Advanced rotated quantization can use distribution-aware calibration and asymmetric query scoring. A paper's algorithm, a vendor's calibrated implementation and an owned approximation are not interchangeable; document what was actually implemented. Lower vector reconstruction error does not by itself guarantee downstream retrieval quality.

Keeping original vectors on disk can lower resident RAM but does not remove them from total storage. Rescoring creates reads, temporary buffers and page-cache pressure. Measure warm and cold queries separately. A codec must include its metadata in the byte budget.

## 8. When to build it: chronological milestones

Milestones are dependency gates, not promised calendar dates. These lanes do not block the basic aKriti ingest, workbench and exact-search MVP.

| Milestone | Prerequisite and work | Exit gate |
|---|---|---|
| R0: contracts and learning | Source/page IDs, proposed vector format, hand-built toy vectors and a float MaxSim exercise | Explain scores, masks, negative maxima and byte budgets; no document-quality claim |
| R1: useful exact retrieval | Stable page renders, aKritiDoc evidence links, owned literal lookup and original-page opening | Exact identifiers retrieve the right source; stale versions and scope boundaries are handled |
| R2: float visual baseline | Working owned visual/query encoder, or explicitly approved compatible open weights; labeled fixture queries | EXP-031 establishes whether visual discovery adds useful recall on our documents |
| R3: compact vector execution | R2 rankings and memory/I/O profile | EXP-032 measures float16/INT8 first, then optional lower-bit scoring and rescoring |
| R4: bounded collection scale | Full-scan reference and a measured latency/energy problem | EXP-033 tests an owned coarse candidate index without unacceptable recall loss |
| R5: product integration | Source resolution, review UX, revisions and cancellation work reliably | LibreOffice uses the owned bounded local interface; Vinti gets a separately scoped adapter later |

**What we can start now:** specify fixtures and format, learn the scorer with synthetic vectors, and build the source-linked exact path when the ingestion contracts are ready. Synthetic vectors test mechanics only, not visual understanding.

**What must wait:** visual-quality results need a real compatible encoder; compressed-vector promotion needs a float baseline; low-bit native kernels need profiling; collection-scale claims need actual scale measurements. Do not silently download a model or start training to fill these prerequisites.

The current roadmap's schema/ingest/workbench/search foundations host R0-R1. The owned module and evaluation lanes host R2. Runtime work hosts R3-R4. LibreOffice and downstream adapters host R5. This is a research overlay, not a replacement for the existing build order.

## 9. A bounded first engineering experiment

Proposed fixture: 100-300 permitted, independently labeled pages and 30-60 queries. Freeze the split and source revisions before comparing systems. Include ordinary text, charts, tables, forms, mixed layouts, poor scans, and pages with missing/broken text layers.

Include Hindi, English and Hinglish queries, names, amounts, dates and visually similar hard negatives. Label relevant page/region and literal evidence separately. These sample counts are planning suggestions, not statistical proof or an already assembled dataset.

| Card | Question | Controlled comparison |
|---|---|---|
| EXP-031-owned-visual-retrieval-baseline | Does page-image retrieval help our documents beyond the owned exact path? | Exact lookup versus exact plus float visual discovery; freeze renders/encoder/query set |
| EXP-032-vector-codec-rescore | How much storage/traffic reduction is safe for our retrieval tasks? | Same float vectors and fixed candidate budget; one precision change; separately test original-vector rescoring |
| EXP-033-coarse-candidate-budget | Can a bounded shortlist avoid expensive full scans without hiding misses? | Same scoring contract; scoped full scan versus owned coarse shortlist; measure candidate omissions first |

An initial evaluation slice can be capped at 30 minutes on one declared device, after explicit authorization and prerequisite availability. Training, data preparation and model downloads require separate budgets. No card has run.

Promotion thresholds must be declared before measurement. Do not adopt a codec because its package is small if it loses critical page recall, increases false certainty, or moves excessive work into disk I/O.

## 10. Measurement and failure gates

| Area | Measurements |
|---|---|
| Discovery | Recall@K, nDCG@K, exact-hit recall, hard-negative errors, candidate omission rate and results by language/page type |
| Evidence | Verified page/region accuracy, literal number/name/character errors, citation correctness and abstention separately from retrieval score |
| Storage | Compressed vectors, retained originals, metadata/coarse index, source/renders and model files reported separately and together |
| Memory | Peak process memory, encoder activations, query buffers, score tiles, mapped/active pages and caches |
| Latency | Query encoding, candidate discovery, MaxSim, rescore reads, source reading/verification; p50/p95 and cold/warm behavior |
| Device cost | Thread count, single-query versus batched load, CPU/GPU use, energy/thermal impact where measurable |
| Integrity | Index/render invalidation, permission changes, deletion, bounded reads, interrupted rebuilds and incompatible embedding pairs |

A retrieval threshold is not calibrated truth confidence. Unsupported claims, ambiguous source characters and conflicting evidence still trigger existing verification or human review.

## 11. LibreOffice and Vinti deployment

**LibreOffice first:** operate on the currently open document, keep persistent indexes optional, cap memory/candidate counts, allow cancellation, and expose progress when indexing is needed. A missing visual model must leave exact search and source viewing usable. Retrieval runs off the UI thread; document writes remain explicit, reviewed actions through the owned integration boundary.

**Larger local collections later:** incrementally index only changed pages, use bounded local storage and introduce a coarse index only after full-scan profiling. Decide whether to retain originals under an explicit storage/quality policy; do not advertise exact float rescoring if the originals were discarded.

**Vinti later:** reuse the general aKriti retrieval contract through a downstream case adapter. Enforce case/access filters before scoring and evaluate legal source fidelity separately. Do not embed Vinti workflow authority inside aKriti retrieval.

Larger offline indexing and smaller online query encoders are possible only with a trained/aligned shared-space pair. Two arbitrary models, or two sizes with the same vector dimension, are not automatically compatible. Query encoder memory remains part of the local device budget even if indexing happened elsewhere.

## 12. Learning concepts and owned engineering value

| Concept | Exercise or implementation role | What to explain before optimizing |
|---|---|---|
| Literal matching and normalization | Owned postings and strict/normalized modes | Why semantic relevance cannot guarantee an exact identifier match |
| Embeddings and similarity | Shared-space page/query vectors | Why vector dimension alone does not establish compatibility |
| Late interaction | Small float MaxSim scorer | Token masking, sum of maxima and negative similarities |
| Memory layout and SIMD | Contiguous page vectors and score tiles | Bandwidth, locality, tiling and why a smaller file may not mean lower peak RAM |
| Quantization and calibration | Float16/INT8 then controlled low-bit codecs | Scale/metadata cost, distortion and asymmetric scoring |
| Candidate indexing | Coarse page representation and shortlist | Recall/latency tradeoff and why rescoring cannot recover missing pages |
| Storage systems | Offset tables, atomic revisions and bounded reads | Cold I/O, mapped-page residency and cache invalidation |
| Source grounding | Evidence resolver and review | Relevance, verified content and action authority are distinct |

The owned engineering opportunity is a source-linked, Indic-evaluated, CPU-conscious retrieval path with measured memory and bounded execution. Combining known methods is not automatically a novel research contribution. Any novelty claim needs prior-art review and a reproducible improvement over the controlled baseline.

## 13. Source assessment and claim discipline

The motivating sources describe page-image multi-vector retrieval, shared-space encoder pairs, calibrated vector quantization and original-vector rescoring. Their measured English retrieval results motivate experiments, not product promises. They do not establish aKriti OCR accuracy, Hindi/Hinglish recall, local encoder latency, or total-device memory.

The detailed named-source assessment and rounded storage example are in the private working note `LOCAL_DOCS/2026-10-14-visual-page-retrieval-and-vector-compression-research.md`. Public implementation contracts use the numbered references and owned module responsibilities.

No external engine dependency, open-weight approval, download, training, test or performance result is created by this planning update.
