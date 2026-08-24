# 10_bitwidth_qat — Low-Bit QAT Curve 

Supports the QAT column added to the manuscript's **Bit-Width Sensitivity** table
(Section IV) and the associated figure.

## File
- `results_lowbit_qat.json` — full-integer (weights + activations) Macro-F1 on the
  CF_9 validation set at 16/8/6/4 bits, for both PTQ and QAT.

## Key numbers
| Bit width | Full-Int. PTQ | Full-Int. QAT |
|-----------|---------------|---------------|
| 16-bit    | 0.8511        | —             |
| 8-bit     | 0.7843        | 0.8493        |
| 6-bit     | 0.5333        | 0.8392        |
| 4-bit     | 0.3997        | 0.6592        |

Interpretation (as in the paper): the sub-8-bit "cliff" is an artifact of PTQ;
QAT recovers the 6-bit full-integer point to near-lossless (0.8392), so INT8 is
selected on hardware grounds (byte alignment, DSP48E2 packing), not because
accuracy forces it. 4-bit remains degraded even under QAT.

The 8-bit QAT validation value (0.8493) is consistent with the deployed test-set
Macro-F1 of 0.8331 (different split: validation vs held-out test day).
