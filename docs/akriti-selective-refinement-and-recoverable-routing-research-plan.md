# Selective Refinement and Recoverable Routing Research

**Recorded:** 2026-10-20  
**Status:** Planned experiments and source assessment; no architecture selection, model adoption or execution.  
**Companion:** [Resumable Colab workflow](akriti-colab-resumable-experiment-workflow.md).

## 1. Scope and current priority

aKriti remains a general-purpose, VLM-first document platform. The CPU text reader is complementary; completing an entire OCR engine is not a prerequisite for every document-VLM experiment. Kriti actions and downstream Vinti authority remain separate from perception and source evidence.

~~~text
aKritiDoc + source identity + transforms + independent fixtures
             |
             +-> document-VLM baseline and structural-reader evaluation
             |
             +-> owned native-text / CPU line-reader control
                         |
             source-linked candidates and review
                         |
             bounded reread / calibrated abstention
                         |
             isolated runtime or routing experiment
~~~

The two baseline tracks share evidence contracts and fixtures, not a mandatory neural architecture. Use owned implementations; any compatible open-weight initialization needs its own approval, role, license and lineage manifest. Nothing here imports an external engine, processor, teacher output, generated label or verifier decision.

## 2. Corrections carried into the research queue

| Point | Required interpretation |
|---|---|
| Real Indic evaluation | Clean rendered examples are useful mechanics controls, not proof of real-scan coverage. Include genuine degraded print, mixed scripts, clipped marks and unreadable cases. |
| Errors have different consequences | Report ordinary/code-point/grapheme errors alongside exact names, digits, dates, amounts, units and IDs. An external LLM severity judge is not canonical ground truth. |
| CTC label units | A shorter BPE transcript does not automatically reduce image/audio feature frames. Compare vocabulary/projection/decoding cost and alignment feasibility separately. |
| Contextual ranking | The frozen-candidate EXP-040 remains a ranking experiment, not permission to generate unsupported repairs. |
| Source recovery | If the true reading is absent from initial candidates, ranking cannot recover it. A bounded reread can generate new candidates from additional original pixels. |
| Repeated agreement | Reused blocks, repeated crops and shared model lineages do not produce independent votes or a correctness certificate. |
| Diffusion | Discrete text decoding and image restoration are distinct experiments. Neither may overwrite the source or turn plausible reconstruction into observed evidence. |

## 3. Candidate coverage and recoverable source access

Preserve the source revision, render/crop transform, original reading, candidates, score semantics and reason for escalation. Any new observation or corrected interpretation becomes a separately attributable event; raw text, typed value and presentation remain distinct.

Measure whether the independently labelled correct reading is present in the first candidate set before scoring the reranker. Report candidate coverage by script and critical-field type, including out-of-vocabulary and unreadable inputs. An oracle candidate choice is only a diagnostic upper bound.

Compare fixed candidates plus contextual ranking against bounded context-preserving source rereading as separate conditions. If detection clipped a vowel mark or merged lines, repair the source window/layout first rather than repeatedly decoding the same defective crop. Include misleading document vocabulary and unknown names.

Audit non-escalated regions as well as selected hard cases. A policy that cheaply misses confidently wrong names or digits has not improved the risk/resource frontier. Calibration, tuning and final evaluation use separate source-grouped splits.

## 4. Recoverable visual-token routing: experiment only

The proposed in-model comparison is full visual processing versus fixed pruning versus recoverable deferral at a matched active-token schedule. Keep the checkpoint, source resolution, prompt/query, precision and evaluation protocol fixed. Deferred tokens must retain positional identity and enough representation to be reconsidered; include that storage and gather/scatter work in accounting.

This is distinct from the existing page/region reread policy. Do not implement token routing before an owned or separately approved VLM baseline exposes the required states. If the architecture cannot support a faithful comparison, park the experiment rather than adopt an external runtime.

Measure full transcription and geometry, not only object localization: small matras/nukta, superscripts, footnotes, stamp fragments, table separators, numbers and chart labels. Include full-token controls and catastrophic omissions. Actual cold/warm latency, process/accelerator peaks and cancellation matter more than theoretical FLOPs alone.

The Reroute paper reports model/schedule-dependent results, including worse moderate-reduction grounding on Qwen3-VL. Recoverable deferral is not universally better, free memory or proof of Indic OCR fidelity. The optimization mechanism remains optional even though preserving source provenance is a platform requirement.

## 5. Diffusion decoding: separate later comparison

LaViDa establishes a multimodal discrete-diffusion research precedent; it does not select aKriti's decoder. A future comparison may examine an owned autoregressive control against an owned masked/diffusion decoder under declared model, training-data and inference budgets. Report any different pretraining or initialization rather than attributing inherited capacity to decoding alone.

First compare simpler bounded rereads and structural readers. A diffusion decoder is eligible only when it can preserve source-linked outputs, exact critical values, ambiguity and structured-artifact validity at useful measured device cost. Fluent text or repaired-looking pixels are not sufficient evidence.

Mercury 3 remains a watch item for language/reasoning latency, not an adopted document reader or local model. Inception's voice cookbook uses external STT/TTS around a Mercury text model; it is not a speech recognizer to transfer into aKriti or Shruti.

## 6. New controlled cards

| Card | Isolated hypothesis and prerequisites | Measurements and stop conditions |
|---|---|---|
| EXP-042-candidate-coverage-recovery | After the fixed reader/EXP-035 control, test whether one bounded source reread recovers readings missing from initial candidates | Candidate coverage, critical exact match, false corrections, accepted risk/coverage and total gate/render/read cost; stop on source/geometry corruption |
| EXP-043-recoverable-visual-routing | After a suitable VLM baseline, vary pruning versus recoverable deferral at a declared active-token budget | Fine-text/structural fidelity, grounding, deferred-state memory, p50/p95 latency and cache cost; no promotion from FLOPs or VQA alone |
| EXP-044-multimodal-diffusion-decoding | After a document-VLM and simpler reread baseline, compare a declared masked/diffusion decoding condition | Exact values, omission/invention rate, aKritiDoc validity, training exposure, latency, steps and total memory; reject unsupported fluent corrections |
| EXP-045-colab-resume-mechanics | After a tiny owned loop, test checkpoint interruption and incomplete publication | Sample/update continuity, parent lineage, complete-generation fallback and budget accounting; record equality/tolerance limits |

Every card is planned and unrun. Declare sample, wall-time/GPU-time, memory and cost limits before separate authorization. Follow [the Colab workflow](akriti-colab-resumable-experiment-workflow.md); no download, paid allocation or benchmark is authorized by this document.

## 7. Metrics and experimental discipline

- Keep raw and normalized readings separate, with declared Unicode and grapheme conventions.
- Report real-scan, mixed-script, structural-mark, critical-value and unreadable slices separately.
- Measure errors among accepted and non-escalated regions, plus review burden and uncertainty intervals; confidence alone is not calibrated risk.
- Include source-resolution changes, gate work, cold model loads, retained deferred states and source I/O in runtime cost.
- Preserve full-source and fixed-policy controls. A cost/benefit ratio alone can hide regressions or become undefined when incremental cost is zero; retain the complete quality/resource table.
- Do not freeze a token budget, retry count, threshold, model family or deployment backend on the strength of this research note.

## 8. Primary sources and disposition

Sources checked for the 2026-10-20 planning assessment; publisher/paper results are not aKriti measurements.

| Source | What transfers | Disposition |
|---|---|---|
| [Devanagari stress test, 2606.29213](https://arxiv.org/abs/2606.29213) | Real scans and structural-error categories matter; post-correction transfer is not assured | Adopt evaluation principles; no external labels or models |
| [Mercury 3 announcement](https://www.inceptionlabs.ai/blog/introducing-mercury-3) | Vendor GPU text-generation result, not local document-model evidence | Watch |
| [Inception voice cookbook](https://docs.inceptionlabs.ai/cookbooks/realtime-voice-assistant) | STT, language response and TTS are distinct responsibilities | Reference only; no APIs adopted |
| [dLLM-ASR, 2601.17902](https://arxiv.org/abs/2601.17902) | Acoustic-prior-guided refinement; reported speed comparison uses large LLM-ASR baselines | Shruti research analogy only |
| [MDM-ASR, 2602.18952](https://arxiv.org/abs/2602.18952) | Masked decoding is a quality/cost comparison, not automatically faster than CTC | Shruti research analogy only |
| [LaViDa, 2505.16839](https://arxiv.org/abs/2505.16839) | Multimodal discrete-diffusion precedent | Later EXP-044; not locked architecture |
| [Reroute, Don't Remove, 2606.12412](https://arxiv.org/abs/2606.12412) | Recoverable visual deferral with architecture/schedule caveats | Later EXP-043; independent implementation only |
| [Colab FAQ](https://research.google.com/colaboratory/faq.html) | Variable resources, interruption-aware planning and cloud I/O boundaries | Workflow reference; no allocation started |

Aurora, intermediate CTC, Indic tokenization and low-bit speech findings are detailed in Shruti's separate research plan. Speech throughput does not establish document quality or choose the aKriti architecture.
