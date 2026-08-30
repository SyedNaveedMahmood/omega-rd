# Scientific contract

This document defines the scientific invariants OMEGA-RD v2 is built to test. It is binding unless explicitly revised in `DECISIONS.md`.

## 1. Primary claim — LOCKED

For patient `p`, gene `g`, and mechanism `m`, OMEGA-RD estimates an observability quantity

`Omega(p,g,m) = integral pi(delta; I_pgm) * w_m(delta) d delta`,

where `pi` is mechanism-specific test power under the realized measurement conditions and `w_m` is an empirical pathogenic effect-size distribution.

Interpretation: `Omega` is the probability that the analysis would reject the null for a pathogenic effect drawn from the mechanism's effect distribution under the sample conditions actually observed.

The graph network is not the primary contribution. It may refine the analytic quantity only under an explicit bound.

## 2. Mechanisms — LOCKED

Initial mechanism vocabulary:

- expression
- splice
- ASE

### Expression

Negative-binomial information for natural-log mean:

`I_expr = s*mu / (1 + phi*s*mu)`.

Effects entered as log2FC are converted internally using `abs(delta) * ln(2)`.

### ASE

At a heterozygous site with depth `N` and beta-binomial intraclass correlation `rho`:

`Var(ratio) = 0.25 * (1 + (N-1)*rho) / N`

and `I_ASE = 1 / Var(ratio)`.

Not heterozygous or zero allelic depth is a hard zero, not low confidence.

### Splicing

For a defined splice event with coverage `C`, baseline usage `psi`, and overdispersion `rho_J`:

`Var(PSI) = psi*(1-psi) * (1 + (C-1)*rho_J) / C`

and `I_splice = 1 / Var(PSI)`.

`psi` must be clipped away from 0 and 1 for the analytic variance calculation.

## 3. Power — LOCKED

Use both terms of the two-sided normal approximation:

`pi(delta; I) = Phi(delta*sqrt(I) - z_(1-alpha/2)) + Phi(-delta*sqrt(I) - z_(1-alpha/2))`.

Do not simplify to the leading term. The zero-information limit must be `alpha`.

## 4. Hard-zero semantics — LOCKED

A structurally unobservable channel emits:

- `Omega = 0`
- `I = 0`
- `MDE = +inf`
- `is_hard_zero = true`
- explicit `reason_code`
- exactly zero evidence points

Examples include unmatched tissue, zero splice coverage, non-heterozygous ASE, zero allelic depth, and missing measurement records.

Raw test power approaching `alpha` as information vanishes must not be confused with the project-level hard-zero representation. Hard zero is a semantic assessment state that yields no evidence.

## 5. Evidence calculus — LOCKED

For evidential conversion use `Omega_eff = max(Omega, alpha)`.

Negative result:

`LR_minus = (1 - Omega_eff) / (1 - alpha)`.

Positive analytic reference:

`LR_plus = Omega_eff / alpha`.

Evidence points use `log(LR) / log(2.08)` with positive cap `+8` and negative cap `-4` by default.

Hard zero therefore has `LR_minus = 1` and exactly zero evidence.

Point/code outputs are proposed and ACMG-compatible; they are not claimed as ClinGen-endorsed or clinically validated.

## 6. Reference uncertainty — LOCKED

Reference moments carry uncertainty. Propagate it through `Omega` with posterior draws (`K=200` default). Report both:

- posterior mean `Omega_mean`
- conservative lower quantile `Omega_q05`

Evidence emission uses the conservative quantile by default.

## 7. Learned residual — LOCKED ARCHITECTURAL CONSTRAINT

If a learned residual is used:

`Omega_star = sigmoid(logit(Omega) + tau * tanh(f_theta(...)))`

with fixed `tau` (default `0.5`). Assert:

`abs(logit(Omega_star) - logit(Omega)) <= tau + 1e-6`.

Hard-zero channels are multiplied by the hard-zero mask after the sigmoid, so they remain exactly zero.

## 8. Abstention — LOCKED INTENT, CALIBRATION PROCEDURE TO BE VERIFIED

The output vocabulary is:

1. EFFECT DETECTED
2. ASSESSED NORMAL
3. NOT ASSESSED

`Omega_min` must ultimately be calibrated on held-out data for false-reassurance risk control. A fixed development threshold must never be presented as the final conformal threshold.

## 9. Claim boundary — LOCKED

Allowed claims concern calibrated detection power/evidence under realized measurement conditions and explicit non-assessment.

Do not claim clinical validation, increased diagnostic yield, superiority at pathogenic-variant detection, or replacement of MRSD/OUTRIDER/FRASER/DROP without experiments specifically supporting those claims.
