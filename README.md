# 12_model_selection — Plain vs Residual, Hardware-Aware Comparison

Supports the FP32/INT8/quantization-loss columns added to the manuscript's
**Residual and Focal-Loss Ablation** table (Section IV), which justify deploying
the residual model over the (nominally more accurate in FP32) plain CNN.

## File
- `results_pcin_compare.json` — three configurations (Plain CNN+CE, Residual CNN+CE,
  Residual CNN+Focal) each run through the identical FP32 → QAT → INT8
  (per-channel-input, weight-folded) flow on CF_9, with per-class INT8 recall/F1.

## Key numbers
| Configuration          | FP32 test | INT8 test | Quant. drop |
|------------------------|-----------|-----------|-------------|
| Plain CNN + CE         | 0.8536    | 0.7691    | 0.0845      |
| Residual CNN + CE      | 0.8471    | 0.8409    | 0.0062      |
| Residual CNN + Focal   | 0.8509    | 0.8331   | 0.0178      |

Interpretation (as in the paper): the plain CNN wins in FP32 but collapses ~8.4 pt
under full-integer quantization, whereas the residual variants stay within ~2 pt;
the residual shortcut provides the quantization robustness the plain topology lacks,
so at INT8 the deployed residual+Focal model outperforms the plain CNN by ~6 pt.

## Note on the deployed-row value (important for cross-checking)
The JSON lists **0.8314** for the Residual+Focal INT8 (this comparison re-runs the
QAT→INT8 flow in a common harness for all three configs). The manuscript's table
reports the **deployment** value **0.8331**, from the exact deployed parameters in
`2_model_weights/int8_params_CF9_f144_pcin.npz`. The two differ only by run-to-run
QAT rounding (≈0.2 pt); both are correct, and the paper uses 0.8331 consistently
with the board test and Table 5. The Plain (0.7691) and Residual+CE (0.8409) values
are the harness runs and appear as-is in the table.
