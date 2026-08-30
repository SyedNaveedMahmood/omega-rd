# OMEGA-RD working instructions

This file is the operational entry point for future implementation work in this repository.

## Authority order

When instructions conflict, use this order:

1. `docs/design/SCIENTIFIC_CONTRACT.md`
2. `docs/design/E1_PROTOCOL.md` for the primary calibration experiment
3. `docs/design/DATA_CONTRACT.md`
4. `docs/design/DECISIONS.md`
5. code/configuration

Do not silently resolve contradictions. Record them in `docs/design/DECISIONS.md` before implementation.

## Non-negotiable rules

- The primary scientific object is analytic observability `Omega`; the graph is subordinate.
- Any structurally unobservable channel contributes **exactly zero evidence**.
- Module C (`omega/observability.py`) remains free of torch/deep-learning dependencies.
- Use the full two-sided power expression; as information tends to zero, raw test power tends to `alpha`, not zero.
- Expression effects specified in log2FC must include the `ln(2)` conversion internally.
- Negative-evidence likelihood ratios use the `alpha` floor.
- Never substitute reference moments from a different tissue; unmatched tissue is a hard zero with a reason code.
- Emit evidence from the conservative posterior quantile (`q=0.05` by default), while reporting the posterior mean separately.
- Hard-zero masks must survive every learned layer and be re-applied after any bounded residual.
- Do not build or optimize the graph model before E1 calibration has passed or its failure has been diagnosed and accepted.

## Experiment discipline

- Separate reference-estimation individuals from confirmatory E1 individuals.
- Pilot and confirmatory E1 results must be distinguished.
- Confirmatory analysis choices are frozen before evaluating the held-out confirmatory cohort.
- Report failures by mechanism and relevant strata rather than hiding them in pooled averages.
- Do not treat technical downsampling replicates as independent biological samples.

## Change discipline

Every scientific change must state which category it is:

- **LOCKED** — required for the paper's claim; changing it requires an explicit design revision.
- **PROVISIONAL** — current implementation choice; may change after QA/pilot work.
- **OPEN** — unresolved; implementation must not pretend it is settled.

For an OPEN issue, prefer a staged analysis artifact over modifying canonical tables.
