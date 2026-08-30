# Implementation sequence

This is the default order for future work. Deviations should be justified in `DECISIONS.md`.

## Phase A — scientific core

1. Implement/reference-test `omega/observability.py` with mechanism-specific information functions and two-sided power.
2. Implement `omega/priors.py` with fitted + shifted + fixed sensitivity variants.
3. Implement `omega/reference.py` uncertainty interfaces.
4. Implement complete power-table builder with explicit hard zeros and provenance.
5. Implement evidence calculus and invariant tests.

## Phase B — reference data and E1

6. Freeze GEUVADIS sample split manifest.
7. Refit expression reference moments on reference-only individuals.
8. Refit ASE overdispersion by stratum.
9. Finalize splice-event representation and shrinkage-aware `rho_J` estimator.
10. Run E1 pilot.
11. Record all changes prompted by pilot in `DECISIONS.md`.
12. Freeze confirmatory protocol/config.
13. Run held-out E1 confirmatory experiment.

### Gate

Do not move to substantive graph optimization unless E1 passes predefined calibration targets or a scientific decision explicitly accepts a characterized failure regime.

## Phase C — evidence and comparisons

14. Validate evidence-point monotonicity/caps/zero behavior.
15. Run E2 MRSD comparison.
16. Implement and validate conformal abstention; run E5.
17. Expand cross-tissue references and run E3.

## Phase D — learned model

18. Build encoders and graph schema.
19. Implement observability-gated constrained attention.
20. Implement bounded residual and hard-zero post-mask.
21. Pretrain/adapt/rank with hard negatives.
22. Run E4 ranking and required ablations.
23. Add counterfactual loss and run E6.

## Phase E — interpretation/release

24. Run E7 retrospective cases.
25. Produce power atlas and evidence calculator artifacts.
26. Re-run all invariant and reproduction tests from clean environment.
27. Manuscript/reporting only after experiment manifests are frozen and archived.
