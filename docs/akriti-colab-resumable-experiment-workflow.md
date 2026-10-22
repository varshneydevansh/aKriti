# Resumable Colab Experiments for aKriti

**Recorded:** 2026-10-20  
**Status:** Workflow specification, not implemented or exercised by this documentation update.  
**Scope:** Optional research compute for owned models and separately approved open-weight initialization. Local deployment remains independently evaluated.

## 1. Compute roles and authorization

| Environment | Purpose | Cannot establish |
|---|---|---|
| Local Mac / RTX 2060 laptop | Learning, debugging, small training controls and target-device profiling | Performance on another user's hardware |
| Colab | Bounded training, architecture ablations and precision experiments | Laptop CPU speed, local privacy, battery life or successful recovery |
| Declared deployment device | Cold/warm inference, full memory, cancellation and sustained interactive behavior | Training feasibility on a different accelerator |

The laptop's 6 GB VRAM is not the training ceiling. Colab also does not make a particular model or full pretraining run feasible by definition: measure the actual accelerator, VRAM, host memory, batch shape and optimizer state first. No GPU type, session duration or quota is assumed.

Before any execution, separately authorize the data/download scope, accelerator use, maximum processed samples/updates, per-session and cumulative GPU time, storage and any paid-compute ceiling. An unset budget is not unlimited permission. Do not purchase units, reconnect automatically or extend a run without authorization.

Colab is optional. A notebook must be a thin launcher for the same repository-owned configuration, data loader, model, training and evaluation entry points used locally. The entry points and artifact layout below are proposed interfaces, not commands that already exist.

## 2. Chronological workflow

| Stage | Proposed work | Required evidence before proceeding |
|---|---|---|
| C0: local mechanics | Source/render identities, geometry fixtures and a tiny training-loop exercise | Explained shapes and source mappings; no quality claim from overfitting |
| C1: run declaration | Freeze hypothesis, control, data splits, precision and budgets | Manifest and source/config/data hashes; cloud eligibility approved |
| C2: environment capture | Record assigned GPU, VRAM, CPU, RAM, OS, driver, CUDA, tensor-library and numerical-library versions | Environment record; no assumed T4/A100 or fixed quota |
| C3: bounded fit pilot | Determine a safe batch/pixel/token budget and checkpoint interval | Pilot treated as budgeted execution, not implicitly authorized by this plan |
| C4: training | One declared change with fixed split and accounted training exposure | Periodic complete checkpoints, progress and resource logs |
| C5: interruption/recovery | Resume only from an eligible complete generation | Recovery evidence under EXP-045, not merely a successful weight load |
| C6: evaluation | Frozen development policy and sealed final test; source-grounded outputs | Paired quality, critical-value, grounding and uncertainty results |
| C7: device promotion | Export an explicitly selected artifact and measure the actual user-device path | Full runtime/package cost and document-fidelity gates |

Stage training shards or archives onto the VM's local disk when appropriate; avoid making a mounted cloud directory the hot path for thousands of tiny reads. Publish durable artifacts deliberately. A Colab-local file is not a durable checkpoint.

## 3. Proposed run artifacts

~~~text
runs/<run-id>/
  run-manifest.json
  environment.json
  data-manifests/
  progress.jsonl
  checkpoints/<generation-id>/
    training-state
    payload-hashes.json
    generation-manifest.json
    COMPLETE
  evaluation/<evaluation-id>/
    manifest.json
    per-source-results.jsonl
    summary.json
~~~

These names specify responsibilities, not a locked serializer or existing implementation. Checkpoints, credentials, data and recordings must not be included in documentation commits. Load only trusted, project-produced training state; do not execute remote model code to recover a run.

## 4. Checkpoint contract

| State | Why it must be captured |
|---|---|
| Experiment/run ID, parent generation and lineage | Prevent results from unrelated runs being merged |
| Code revision or immutable source snapshot, config and package versions | Reconstruct the actual computation, including uncommitted source when applicable |
| Model tensors, buffers and parameter-sharing topology | Restore learned state without silently duplicating or untying shared weights |
| Optimizer, scheduler, mixed-precision scaler or explicit disabled state | Continue training, not merely restart from weights |
| Epoch, completed optimizer steps, examples/pixels/tokens consumed and budget usage | Preserve training exposure and budget accounting |
| Python, NumPy, tensor CPU and relevant GPU RNG states | Reconstruct stochastic operations as far as the declared environment permits |
| Sampler order/state, shard identity and committed data cursor | Avoid unintended omission or replay of data |
| Augmentation policy/state and preprocessing/cache fingerprints | Prevent a new input distribution being disguised as a resume |
| Train/development/calibration/test manifests and normalizer versions | Keep tuning and final evaluation separated |

The first implementation should checkpoint at a defined optimizer-step boundary: complete the accumulated update and declared scheduler/scaler transitions, clear gradients, then snapshot state before consuming the next batch. Do not serialize mutable tensors while the optimizer is changing them.

If an interruption occurs during gradient accumulation, discard that partial update and return to the last complete boundary. Mid-accumulation recovery would additionally require gradients and exact microbatch state; it is outside the first workflow.

An iterator index alone is insufficient with worker prefetch or stochastic augmentation. Prefer a reproducible sample-order plan, an acknowledged post-update cursor and explicit augmentation seeds/state. If that contract is unavailable, declare epoch-boundary recovery and its possible reprocessing instead of claiming exact mid-epoch continuation.

## 5. Durable publication and fallback

1. Create a unique immutable generation from a consistent local snapshot. Never overwrite the only usable checkpoint.
2. Write all payloads, calculate lengths/hashes and upload them to the approved durable location.
3. Read back or otherwise validate the published payloads during an authorized run. Publish the generation manifest and COMPLETE marker last.
4. A resume reader must require the marker, manifest compatibility and matching payload hashes. A latest pointer is a convenience, not proof of completeness.
5. Retain at least one preceding complete generation within the agreed storage budget. On partial publication, fall back explicitly and report lost updates.

Do not assume rename or atomic-write guarantees across a mounted Drive filesystem, synchronization layer or interrupted upload. The consumer validates a generation; existence of a file or marker alone is insufficient.

## 6. Recovery versus a new branch

**Compatible continuation:** restore all eligible training/data state under the pinned configuration and check the declared batch order and next update. Even the same hardware and seeds do not automatically guarantee bitwise-identical operations.

**Cross-hardware continuation:** record a parent-linked child run when accelerator, numerical backend, precision, effective batch, software or accumulation behavior changes. Restore compatible state deliberately, report changes and rerun development checks under a separately approved budget. Do not silently change BF16 to FP16, reset the scheduler or reinterpret incompatible optimizer state.

**Weights-only restart:** label it as initialization or a new training branch. It is not a complete resume.

Store tensors in a portable representation where possible, but validate architecture, vocabulary, shared parameters and optimizer compatibility. Portability does not prove numerical equivalence.

## 7. Data and privacy gates

- Upload only allowlisted, rights-cleared and explicitly cloud-eligible data. Public availability is not sufficient permission by itself.
- Do not upload private court files, personal documents, source crops or corrections by default. Separate local evidence retention from training eligibility.
- Keep page/source groups and their derived crops in the same split. Freeze separate calibration and final-test manifests; do not optimize on final-test outcomes.
- Avoid broad Drive access when a dedicated approved transfer scope suffices. Never put credentials or private document text in notebook outputs or checkpoint metadata.
- Record remote retention/deletion responsibilities. Deleting a local copy does not delete a cloud copy or remove its contribution from a trained checkpoint.
- External OCR/VLM implementations, processors, engines, dictionaries, teacher outputs and generated labels remain outside the approved pipeline. Ordinary tensor/image/numerical libraries are plumbing; any open-weight base needs a separate manifest and approval.

## 8. Proposed recovery laboratory: EXP-045

After a runnable owned toy loop exists, compare an uninterrupted control with interruption at a safe boundary, interruption during accumulation and incomplete durable publication. Check next sample/update accounting, fallback behavior, cumulative budget and resulting losses/parameters within declared tolerances. Test a hardware-change branch separately when available; do not require paid hardware to complete the first lab.

This is a planned fault-recovery experiment, not a test executed by writing this document. A recovery report must distinguish exact equality, tolerance-based agreement and merely successful continuation.

## 9. Evidence and related plans

Google documents variable Colab resource availability and runtime limits, as well as Drive I/O considerations: [Colab FAQ](https://research.google.com/colaboratory/faq.html), accessed 2026-10-20. No quoted GPU quota or session duration is a project assumption.

Read [the implementation roadmap](akriti-implementation-plan.md), [selective refinement and recoverable routing](akriti-selective-refinement-and-recoverable-routing-research-plan.md), and [the experiment matrix](akriti-research-to-experiment-matrix.md). Colab results cannot substitute for local CPU, Apple Silicon, browser or LibreOffice deployment evidence.
