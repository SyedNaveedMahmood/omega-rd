# E1 protocol — primary calibration experiment

Status: **DESIGN FREEZE TARGET**. Pilot implementation may expose defects, but changes made after pilot inspection must be documented before confirmatory evaluation.

## 1. Question

When OMEGA-RD predicts observability `Omega = x` under realized measurement conditions, does the corresponding mechanism detector identify a pathogenic-sized effect with empirical frequency approximately `x`?

E1 tests the measurement model, not clinical diagnosis.

## 2. Primary endpoints — LOCKED

Primary calibration endpoints:

- Expected calibration error (ECE), target `< 0.05`
- reliability/calibration slope, target `[0.9, 1.1]`

Also report calibration intercept and Brier score with uncertainty intervals.

A pooled passing result does not erase mechanism- or stratum-specific failure.

## 3. Cohort separation — PROVISIONAL NUMBERS, LOCKED PRINCIPLE

The same individual must not contribute both reference-moment estimation and confirmatory calibration evaluation.

Initial GEUVADIS split recommendation for 464 mapped individuals:

- 60% reference estimation (~278)
- 20% development/pilot (~93)
- 20% locked confirmatory (~93)

The exact split may change before the first confirmatory read. Seed and membership lists must be materialized and versioned.

## 4. Measurement degradation — LOCKED LEVELS

Evaluate depth fractions:

- 50%
- 25%
- 10%
- 5%
- 2%

Use `B = 50` independent technical replicates per rung for the confirmatory experiment unless a documented compute/power analysis revises this before confirmatory execution.

Technical replicates are not independent biological observations; uncertainty calculations must cluster/resample at the individual level.

## 5. Two-tier execution — PROVISIONAL

### Pilot

Use count/binomial thinning of processed expression, junction, and ASE counts. Purpose: expose formula, implementation, and reference-model failures cheaply.

### Confirmatory subset

Where practical, perform read-level downsampling for a representative subset to test quantification/mapping effects omitted by count thinning. The count-level experiment remains useful; the read-level subset is an robustness check.

## 6. Effect injection — LOCKED PRINCIPLE

At each mechanism/rung draw effect magnitude `delta` from the pre-specified effect prior variant being evaluated. The detector must not receive the injected `delta` as an input to `Omega`; `Omega` is computed only from realized measurement conditions and the frozen prior.

Mechanism-specific injection must preserve the appropriate generative model:

- expression: NB-consistent mean shift in log2FC units
- ASE: allelic-ratio departure at a heterozygous site under beta-binomial sampling
- splice: event-level PSI shift under an overdispersed binomial/beta-binomial model

## 7. Effect-prior sensitivity — LOCKED SET

Evaluate at least:

1. fitted prior
2. 25% smaller-effect shift
3. 25% larger-effect shift
4. fixed-effect fallback (`1.0` log2FC expression, `0.15` ASE departure, `0.10` delta-PSI)

Report how calibration and downstream assessment/abstention decisions move under these variants.

## 8. Mechanism-specific diagnostic strata — LOCKED REPORTING INTENT

### Expression

At minimum stratify by mean expression, dispersion, and depth/size factor.

### ASE

At minimum stratify by allelic depth, expression/depth stratum, and `rho_ASE`.

### Splicing

At minimum stratify by event coverage, baseline PSI, and `rho_J`.

## 9. Splice event definition — OPEN

Confirmatory splice E1 must run at a defined event level before any gene-level aggregation is evaluated.

Current candidate connected-component PSI construction is QA only and is not the canonical definition.

Preferred next candidate: strand-aware donor- and acceptor-anchored competing-junction events, using all observed junctions as denominator/background competitors while promoting only sufficiently recurrent/qualified targets.

The rule for marginalizing multiple splice events into gene-level `Omega(p,g,splice)` remains OPEN and must be resolved separately. Do not use max-event `Omega` as a silent default.

## 10. Reference model requirements — LOCKED

Before confirmatory E1:

- reference/evaluation individuals are disjoint
- expression reference moments include posterior uncertainty
- `rho_ASE` is stratified rather than a single global constant
- splice `rho_J` is estimated from sample-level recount3 junction data
- boundary-heavy raw `rho_J=0` estimates are not accepted without likelihood/shrinkage QA
- tissue matching is exact; no cross-tissue fallback

## 11. Statistical reporting — LOCKED PRINCIPLE

Report:

- calibration plot: predicted `Omega` vs empirical detection frequency
- ECE
- calibration slope + intercept
- Brier score
- confidence intervals from individual-clustered bootstrap/resampling
- per-mechanism results
- pre-specified failure strata

Do not calculate confidence intervals treating repeated depth/injection replicates as independent subjects.

## 12. Failure policy — LOCKED

If E1 misses its target, stop downstream model escalation and diagnose by mechanism/stratum. Likely failure modes include low-count normal approximation, misspecified overdispersion, reference mismatch, and effect-prior misspecification.

A characterized failure regime is reportable; hiding it with a learned model is not acceptable.
