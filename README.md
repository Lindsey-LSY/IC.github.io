# Supplementary Material for Review

**"Microsecond-Latency, Fully On-Chip Limit Order Book Prediction on an Edge FPGA:
A Lightweight INT8 1D-CNN Hardware–Software Co-Design"**

This package provides evidence supporting the manuscript's claims
(data protocol, hardware synthesis/place-and-route, on-board verification, quantized
parameters). 
---

## Provided (for the reviewers)

| Folder | Contents |
|--------|----------|
| `1_qat_config/` | `hyperparams_table3.md` — full QAT/training hyperparameter table (optimizer, lr, epochs, seed, dropout, etc.) | 
| `2_model_weights/` | Trained parameters: `variant_k3_f144_tail_CF9.pth` (FP32), `best_qat_pcin_CF9_f144.pth` (QAT), `int8_params_CF9_f144_pcin.npz` (fixed-point), `results_int8_pcin_CF9_f144.json` |
| `3_hls_code/` | Per variant: `weights.h` (exported INT8 weight arrays), `lob_tb.cpp` (HLS **test bench** with golden I/O), `hls_config.cfg`, `vitis-comp.json`  | 
| `4_dataset_manifest_and_leakage/` | `dataset_manifest_CF1..9.{csv,json}`, `leakage_check_CF1..9.log`, `test_set_access_audit_step{2,3}.log`   | 
| `5_normalization/` | `NORMALIZATION.md` (causal Z-score, Eq. 7) | 
| `6_seeds/` | `SEEDS.md` (3407 / 2026 / 5-seed set) | 
| `7_board_test/` | Full-test-set on-board verification: `board_manifest.csv` (all **31,828** on-board sample IDs), `board_golden.npz` (golden outputs), `board_paired_results.csv` (31,828 FP32/INT8/board pairs), `mcnemar_result.txt` (McNemar FP32-vs-INT8), `board_test_144.py` (host **test** driver), `board_test_provenance.log` | 
| `8_postroute_reports/` | Vivado routed `timing_summary` / `utilization_placed` / `power_routed` reports for the 5 feasible folding variants on xcku040-ffva1156-2-e | 
| `9_across_fold/` | All-nine-fold generalization: FP32 and QAT-INT8 per-fold Macro-F1 + mean±std. See folder `README.md` for the CF_9 value note. |
| `10_bitwidth_qat/` | Low-bit QAT curve: full-integer PTQ vs QAT at 16/8/6/4 bits. |
| `11_latency_and_baselines/` | Batch-one latency distributions for GPU/CPU + optimized ONNX Runtime CPU baseline. |
| `12_model_selection/` | Plain-vs-residual hardware-aware comparison: FP32/INT8 test Macro-F1 and quantization drop for the three configurations. See folder `README.md` for the deployed-row value note. |
| `13_power_measurement/` | Direct whole-board power measurement: DT550 in-line reading of 3.11 W for the deployed LO-Arch. |


---

## Notes
- **Main fold** CF_9 (train days 1–9, test day 10): training pool 325,069 samples,
  test set 31,828; purge/embargo T+k = 110 ticks at every boundary.
- **Deployed model:** 144-dim input, K=3, 86,211 parameters; FP32 Macro-F1 0.8509 →
  INT8 (per-channel input, weight-folded) 0.8331.
- **On-board verification:** the full 31,828-sample CF_9 test day is streamed on
  board and is **bit-exact** with the PyTorch INT8 reference (31,828/31,828);
  board accuracy 83.12 % (95 % Wilson 82.71–83.53 %).
- **Board:** ALINX AXKU041 (xcku040-ffva1156-2-e), 100 MHz, JTAG-to-AXI.
- **Power / energy efficiency:** deployed LO-Arch whole-board power is directly
  measured at 3.11 W (DT550 in-line, 12 V rail); with 57 µs latency this gives a
  10.44× latency reduction and 41.6× energy-efficiency improvement over the RTX
  5070 Laptop GPU baseline. The chip-level values in `8_postroute_reports/` are
  Vivado vectorless estimates on a different basis (see `13_power_measurement/`).
- All `weights.h` across the HLS variants are identical (same INT8 parameters);
  variants differ only in loop folding / stream count.
- **Value-provenance note:** the paper reports the deployed CF_9 INT8 Macro-F1 as
  0.8331 throughout (board-aligned). Two supporting sweeps re-run the QAT→INT8
  flow independently and list 0.8300 (`9_across_fold/`) and 0.8314
  (`12_model_selection/`); the ≈0.2–0.3 pt difference is run-to-run QAT rounding,
  explained in each folder's `README.md`.

