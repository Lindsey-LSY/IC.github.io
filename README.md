# 14_sota_rerun — Same-Protocol SOTA / Baseline Reruns

Supports the comparable zone (a) of the manuscript's **Positioning Against Prior
LOB Predictors** table (Table SOTA, Section VI-A): DeepLOB, TLOB, MLP and LSTM,
all re-run in this work under **one identical leakage-controlled protocol** so that
their Macro-F1 values are directly comparable to the proposed model.

## Protocol (identical for all four baselines and the proposed model)
- FI-2010 NoAuction Z-score, **144-dimensional** input, three-class labels, k=100.
- Anchored fold **CF_9** (train days 1–9, test day 10), purge/embargo T+k=110 ticks.
- Same normalization and chronological split as the proposed model.
- Test set evaluated **once** per model (see `test_set_access_audit.log`, one
  timestamped line per model).
- Only the network architecture differs; each baseline keeps its **published
  architecture** but is trained/evaluated under this common protocol rather than
  its original paper's setup.

## Files & headline numbers (test day, CF_9)
| File | Model | Macro-F1 | Accuracy |
|------|-------|----------|----------|
| `results_deeplob_CF9_f144.json` | DeepLOB (conv + Inception + LSTM) | 0.8789 | 0.8791 |
| `results_tlob_CF9_f144.json`    | TLOB (dual attention)            | 0.8417 | 0.8398 |
| `results_baseline_mlp_CF9_f144.json`  | MLP                        | 0.8243 | 0.8217 |
| `results_baseline_lstm_CF9_f144.json` | LSTM                       | 0.7986 | 0.7984 |

Each JSON includes the full hyperparameters, the per-epoch training history, the
best epoch, per-class precision/recall/F1, and the confusion matrix.

## Notes on scope
- **TLOB:** its original work adopts its own labeling scheme. Here the TLOB
  *architecture* is reused but evaluated under the **shared FI-2010 label protocol**
  of this study, so the reported Macro-F1 reflects the common protocol, not TLOB's
  native labeling. The final test metrics were read once from the best saved
  checkpoint (best epoch 59); the final evaluation was re-run from that checkpoint
  after a `torch.load` `weights_only` API incompatibility (older PyTorch) was
  resolved, and the full 100-epoch training history is included in the JSON.
- **DeepLOB:** the recurrent LSTM front-end is retained; only the input front-end
  is adapted to the 144-dimensional feature vector.
- These reruns are the source of the comparable-zone values in Table SOTA; the
  literature values quoted in the non-comparable zone come from the original papers
  under different protocols and are not reproduced here.
