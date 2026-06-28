# Gate 3 Deliverable: Evaluation Metrics

## Evaluation Setup

Dataset:

```text
SDNET2018 small subset
Path used locally: D:\VuXuanBach\AI thuc chien\build phase\eval
```

Model and run settings:

| Item | Value |
|---|---|
| Provider | Novita |
| Model | `qwen/qwen3-vl-235b-a22b-instruct` |
| Task | Image-level crack detection |
| Labels | `Cracked` vs `Non-cracked` |
| Target class | `crack` |
| Images evaluated | 168 |
| Detail | `low` |
| Grid | `1` |

Output artifact in the source project:

```text
surface_inspection_product/vlm_surface_inspection/runs_eval_sdnet/evaluation_qwen3vl235b_sdnet_subset.json
```

## Main Metrics

| Metric | Value |
|---|---:|
| True Positive | 35 |
| False Positive | 0 |
| True Negative | 84 |
| False Negative | 49 |
| Accuracy | 70.83% |
| Precision | 100.00% |
| Recall | 41.67% |
| Specificity | 100.00% |
| F1 | 58.82% |
| Average latency | 6.47 sec/image |
| Min latency | 2.20 sec |
| Max latency | 21.57 sec |
| Total requests | 234 |
| Input tokens | 269,595 |
| Output tokens | 32,484 |
| Novita dashboard cost | $0.1148 total |

## Baseline Comparison

Gate 3 asks for more than one metric with a baseline number. This evaluation
compares the VLM against two simple baselines.

| Model / Baseline | Accuracy | Precision | Recall | Specificity | F1 |
|---|---:|---:|---:|---:|---:|
| Qwen3-VL inspection run | 70.83% | 100.00% | 41.67% | 100.00% | 58.82% |
| Always predict no defect | 50.00% | 0.00% | 0.00% | 100.00% | 0.00% |
| Always predict defect | 50.00% | 50.00% | 100.00% | 0.00% | 66.67% |

## Interpretation

- The VLM improves accuracy over both naive baselines.
- `FP=0` means no non-cracked images were incorrectly marked as cracked in this
  subset.
- Precision and specificity are strong on this subset.
- Recall is the main weakness: `FN=49`, so many cracked images were missed.
- The result is good enough for an MVP screening workflow, but not enough for
  fully automated pass/fail quality certification.

## Manual Evidence Cases

| Case | Input image | Expected | Actual VLM output | Result type | Latency |
|---|---|---|---|---|---:|
| 1 | `Decks/Cracked/7001-2.jpg` | crack | `suspected_defect`, 1 crack, confidence 0.85 | TP | 10.94s |
| 2 | `Decks/Cracked/7001-21.jpg` | crack | `suspected_defect`, 1 crack, confidence 0.90 | TP | 11.12s |
| 3 | `Decks/Non-cracked/7001-1.jpg` | no crack | `no_defect`, `defects=[]` | TN | 7.72s |
| 4 | `Decks/Non-cracked/7001-10.jpg` | no crack | `no_defect`, `defects=[]` | TN | 7.14s |
| 5 | `Decks/Cracked/7001-115.jpg` | crack | `no_defect`, `defects=[]` | FN | 6.92s |

## Cost and Latency Notes

- The average latency was about 6.47 seconds per image.
- The full subset run cost reported by the Novita dashboard was $0.1148.
- The MVP uses `detail=low` and small grid settings for demo cost control.
