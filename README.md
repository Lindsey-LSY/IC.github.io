# 13_power_measurement — Direct Board Power Measurement

Supports the manuscript's switch from a Vivado vectorless **estimate** to a
**directly measured** whole-board power for the deployed LO-Arch (Section VI):
**3.11 W** (fluctuating within 3.10–3.12 W), which sets the 41.6× energy-efficiency
ratio against the GPU.

## Measurement setup
- Instrument: external DT550 in-line DC power meter, on the board's 12 V supply rail.
- Board: ALINX AXKU041 (xcku040-ffva1156-2-e), 100 MHz.
- Reading basis: whole-board draw during sustained on-board inference (same
  device-level basis as the GPU total-card power, so the two are comparable).

## Files
- `power_measurement.mp4` — video of the DT550 in-line meter during sustained
  on-board inference, showing the whole-board power holding at **3.10–3.12 W**
  (reported as 3.11 W in the manuscript).

## Relationship to Fig. (power breakdown) and Table (folding DSE)
The per-configuration chip-level values (LO-Arch 2.288 W, Sweet-spot 1.926 W,
Uniform-8 1.005 W) in the folding table and the power-breakdown figure are Vivado
post-route **vectorless chip-level estimates**, used only for relative comparison
within the folding family. They are a different basis from the whole-board DT550
measurement (3.11 W) and are not directly comparable in absolute terms; see
`8_postroute_reports/` for the underlying `power_routed.rpt` files.
