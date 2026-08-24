# 11_latency_and_baselines — Latency Distributions & Optimized CPU Baseline

Supports the manuscript's **Batch-One Latency Distribution** table (Section VI)
and the ONNX Runtime baseline discussion.

## Files
- `results_latency_dist.json` — batch-one latency over 5,000 iterations for the
  GPU (RTX 5070 Laptop, FP32) and CPU (PyTorch, FP32): median / p95 / p99 / min /
  max / std, plus GPU batch-4 per-sample latency. FPGA core is deterministic
  (fixed cycle count, σ = 0, 57 µs).
- `results_onnx_baseline.json` — optimized CPU baseline (ONNX Runtime,
  graph-optimized): prediction-level agreement with PyTorch = 100 % (identical
  Macro-F1 0.8509), median 225.8 µs, p95 327 µs, p99 535 µs.

## Key numbers (per-sample, batch = 1)
| Platform            | Median (µs) | p95 | p99 | Std |
|---------------------|-------------|-----|-----|-----|
| GPU (FP32)          | 563         | 751 | 995 | 91  |
| CPU (PyTorch, FP32) | 288         | 466 | 560 | 64  |
| CPU (ONNX RT, FP32) | 226         | 327 | 535 | 79  |
| FPGA core (INT8)    | 57          | 57  | 57  | 0   |

Note: the distribution-run GPU mean (588 µs) is consistent with the 594.96 µs
steady-state value used for the energy-efficiency ratios in the paper (two
measurement runs of the same platform). ONNX Runtime is a native-CPU optimized
baseline and is a different measurement basis from the PyTorch-CPU row (288 µs).
