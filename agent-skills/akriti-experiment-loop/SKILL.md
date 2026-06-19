---
name: akriti-experiment-loop
description: Use when planning, designing, or running controlled aKriti model experiments, ablations, training loops, fine-tuning, distillation, quantization, OCR/VLM evals, or Karpathy-autoresearch-style autonomous research cycles.
---

# aKriti Experiment Loop

This skill turns open research ideas into controlled aKriti experiments.

## Rule

Do not start training, large downloads, paid cloud jobs, or long benchmarks unless the user explicitly asks. By default, produce the experiment plan, commands, metrics, and logging structure.

## Loop

Use a fixed-budget loop inspired by `karpathy/autoresearch`:

1. State one hypothesis.
2. Choose one benchmark slice.
3. Choose one measurable target.
4. Change one variable.
5. Run for a fixed wall-clock/token/sample budget only if approved.
6. Compare against the previous baseline.
7. Keep, revert, or park the change.
8. Log the result with enough detail to reproduce it.

## aKriti benchmark slices

Choose from:

- OCR: CER/WER, Indic script CER, layout-aware text order, formula/footnote fidelity.
- Layout: block IoU, reading order accuracy, page segmentation F1.
- Tables: cell detection F1, row/column structure accuracy, CSV/HTML reconstruction accuracy.
- Charts: chart type accuracy, axis/legend extraction, data-series reconstruction, chart QA.
- Images/thumbnails: semantic caption quality, safety/category labels, low-latency thumbnail triage.
- Translation: chrF, BLEU, COMET-style reference checks, terminology consistency, layout preservation.
- Document edits: patch correctness, provenance coverage, destructive-edit prevention.
- Retrieval: exact citation recall, semantic recall, false-positive rate, page/region grounding.
- Runtime: latency, peak RAM/VRAM, tokens/sec, image/page/sec, package size, battery/CPU load.

## Device targets

Use these profiles when sizing experiments:

- RTX 2060 6GB: tiny/core prototyping, quantized inference, small LoRA/QLoRA, OCR/layout experiments.
- Mac M4 24GB: MLX/Core ML/local runtime checks, CPU/GPU mixed inference, app UX prototypes.
- Browser/WebGPU: FilterTube thumbnails, local semantic filtering, tiny VLM/classifier paths.
- Cloud H100/H200/Blackwell: teacher runs, serious fine-tuning, distillation data generation, large eval sweeps.

## Log template

Write or propose entries in this shape:

```markdown
## EXP-{YYYYMMDD}-{slug}

- Hypothesis:
- Baseline:
- Change:
- Dataset slice:
- Budget:
- Metrics:
- Result:
- Decision: keep | revert | park
- Notes:
```

## Guardrails

- Prefer small ablations over broad “try everything” runs.
- Keep a held-out eval set that the agent never optimizes directly.
- Separate generation quality from verification quality.
- Never accept an OCR/VLM improvement without checking hallucination and provenance failure rate.
- Treat benchmark gains as provisional until they survive a second dataset slice.
