# Experiment map

## Stage 0 — reference-model QA

Purpose: establish trustworthy `mu, phi, psi, rho_J, rho_ASE` and posterior uncertainty before calibration.

Gate: no cross-tissue fallback; disjoint reference/evaluation membership; explicit provenance; hard-zero tests pass.

## E1 — downsampling calibration — LOAD-BEARING

Primary question: does predicted `Omega` equal empirical detection frequency under controlled degradation of measurement conditions?

Run pilot first, freeze analysis choices, then run held-out confirmatory evaluation. See `E1_PROTOCOL.md`.

## E2 — comparison with MRSD

On splicing, compare assessability/feasibility calls against empirical E1 detection frequencies. Pre-specified qualitative hypothesis: agreement in well-behaved junctions; divergence where coverage looks adequate but overdispersion makes detection weak.

Do not frame MRSD as an inferior obsolete method; position OMEGA-RD as extending coverage feasibility with dispersion and effect-size distributions.

## E3 — cross-tissue observability

Use deliberately selected recount3 tissues to test whether OMEGA-RD orders gene assessability sensibly across accessible tissues. This experiment waits until the LCL reference/calibration pipeline is stable.

## E4 — ranking

Metrics: MRR, nDCG@K, Recall@K, Top-1/Top-5; mechanism precision/recall and macro-F1.

Baselines include single-channel scores, variant-prior only, naive fusion, and standard heterogeneous graph without observability gating.

Ranking is necessary but not the scientific headline.

## E5 — abstention

Evaluate selective prediction, coverage vs accuracy, and realized false-reassurance risk versus nominal conformal target. Report abstention fraction by tissue/gene expression strata.

## E6 — counterfactual faithfulness

If mechanism `m*` is credited, removing its evidence must reduce the score more than removal of alternative mechanisms. Counterfactual masking removes mechanism evidence while retaining the observability node.

## E7 — retrospective public solved cases

Illustrative case analysis only. Report where the framework would assign graded benign evidence, abstain, or materially differ from fixed-strength published interpretation. Do not call this clinical validation.

## Mandatory ablations

At minimum retain:

- `tau=0` analytic-only
- fixed effect size vs marginal effect prior
- no alpha clamp
- Omega plain feature/no gate
- unconstrained gate signs
- no hard-zero mask
- no posterior draws
- no counterfactual loss
- no hard negatives

Ablations must be interpreted in terms of which scientific property they test, not only performance change.
