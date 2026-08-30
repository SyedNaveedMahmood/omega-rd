# Decision log

Use this file for scientific/architectural decisions that affect interpretation. Each entry should identify status and rationale.

## D001 — Analytic observability is primary

**Status:** LOCKED

`Omega` is the primary scientific object. Learned graph components are subordinate and cannot replace failed analytic calibration.

## D002 — Exact zero evidence for structural non-observability

**Status:** LOCKED

Hard-zero channels remain exactly zero through evidence conversion and learned refinement.

## D003 — E1 precedes graph-model optimization

**Status:** LOCKED

Do not spend substantive effort optimizing ranking/graph/MoE architectures until the analytic calibration experiment has passed or its failure regime has been formally characterized.

## D004 — Separate reference and confirmatory individuals

**Status:** LOCKED PRINCIPLE

Reference-moment estimation and confirmatory E1 evaluation use disjoint individuals. Exact split is provisional until materialized.

## D005 — Initial confirmatory split recommendation

**Status:** PROVISIONAL

For 464 GEUVADIS individuals: approximately 60% reference / 20% pilot / 20% confirmatory, using a pinned seed and versioned membership manifest.

## D006 — Splice QA connected-component PSI is not canonical

**Status:** LOCKED REJECTION OF CURRENT QA AS FINAL

The current connected-component junction clustering was useful for data QA but can create transitive competitors. It must not silently become the final PSI/event definition.

## D007 — Candidate splice-event representation

**Status:** PROVISIONAL

Evaluate strand-aware donor- and acceptor-anchored competing-junction events. Include all observed junctions as potential competitors/background; qualify target events separately.

## D008 — Gene-level splice observability aggregation

**Status:** OPEN

The original patient x gene x mechanism power-table contract does not fully specify aggregation over multiple splice events. Event-level `Omega(p,g,j,splice)` should be calibrated first. Do not use max/min aggregation without an explicit decision and validation.

## D009 — `rho_J` estimation

**Status:** PROVISIONAL DIRECTION

Simple method-of-moments estimates truncated at zero are insufficient for the confirmatory reference model. Next implementation should evaluate beta-binomial likelihood estimation plus shrinkage/uncertainty across events.

## D010 — MoE

**Status:** DEFERRED

A standard Mixture-of-Experts is not a headline novelty contribution. If tested later, it should be observability-constrained, preserve hard-zero routing, remain inside the bounded residual, and be rejected if it improves ranking at the expense of calibration.

## D011 — Current fixed `omega_min=0.2`

**Status:** DEVELOPMENT ONLY

Any fixed threshold in the smoke build is provisional. Final `Omega_min` must come from held-out conformal/risk calibration; do not report `0.2` as a final scientific threshold.

## D012 — Current smoke dataset

**Status:** DEVELOPMENT ONLY

The 40-patient/10-gene processed DuckDB is appropriate for schema and invariant testing but not final E1 confirmation or final graph-model evaluation.
