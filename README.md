# OMEGA-RD v2

**Observability-Modelled Evidence Grading for Rare Disease**

OMEGA-RD v2 is a methods/benchmark project for quantifying what a transcriptomic result is worth given whether the relevant biological mechanism was actually observable in that sample.

The primary scientific object is the observability operator `Omega(p,g,m)`: marginal detection power for patient `p`, gene `g`, and mechanism `m`, integrated over an empirical pathogenic effect-size distribution.

## Status

Early implementation and experiment-design phase. The analytic operator and its calibration are the load-bearing contribution; graph modelling is downstream.

## Repository map

- `omega_rd/configs/` — canonical configuration and panels
- `omega_rd/data/` — downloaders, preprocessing, annotations
- `omega_rd/omega/observability.py` — Module C, analytic observability; must remain torch-free
- `omega_rd/omega/reference.py` — Module B reference moments and uncertainty
- `omega_rd/omega/priors.py` — pathogenic effect-size priors and sensitivity variants
- `omega_rd/omega/evidence.py` — likelihood-ratio/evidence-point calculus
- `omega_rd/omega/conformal.py` — abstention risk control
- `omega_rd/omega/evaluation/` — E1-E7 experiment harnesses
- `omega_rd/omega/simulation/` — downsampling and injection utilities
- `tests/` — invariant tests that protect scientific claims
- `docs/design/` — binding implementation and experiment contracts

## Read before changing scientific code

1. `AGENTS.md`
2. `docs/design/SCIENTIFIC_CONTRACT.md`
3. `docs/design/E1_PROTOCOL.md`
4. `docs/design/DATA_CONTRACT.md`
5. `docs/design/DECISIONS.md`

The design files intentionally distinguish **LOCKED**, **PROVISIONAL**, and **OPEN** decisions. Do not silently convert an open question into an implementation choice.

## Claim discipline

This repository supports a methods + benchmark paper on public data. It is **not a clinical diagnostic device**, and evidence-point mappings are proposed/ACMG-compatible rather than clinically validated.
