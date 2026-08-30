# Data contract

This document defines the canonical data interfaces and current development-data status.

## 1. Current development assets

Current smoke build characteristics known from the August 2026 processed run:

- 10 panel genes
- 40 GEUVADIS-derived development patients
- 12 recount3 K562 references in the smoke database
- 1,200 power rows = 40 x 10 x 3 mechanisms
- 1,200 evidence rows
- 13,703 graph nodes
- 45,726 graph edges

The smoke database is a development artifact, not the confirmatory evaluation dataset.

Additional verified downloads now include:

- gnomAD v4.1.1/Ensembl LOEUF GRCh38 dataset
- recount3 ERP001942 GEUVADIS junction MM/RR/ID files
- recount3 ERP001942 metadata sidecars

A staged junction QA run mapped 667 recount3 runs to 464 GEUVADIS individuals and found 489 annotated panel junction mappings. Candidate baseline files are QA artifacts only until the splice-event definition is frozen.

## 2. Canonical tables

### panel

`panel_gene, gene_symbol, chrom, gene_start, gene_end, strand, gencode_id, candidate_region_start, candidate_region_end`

### reference_moments

`panel_gene, tissue, median_tpm, mean_tpm, var_tpm, nb_dispersion, fraction_expressed, n_samples, mean_posterior_var, disp_posterior_var, provenance_flag`

### junction_baseline

Current canonical target:

`panel_gene, junction_id, tissue, baseline_psi, psi_overdispersion, mean_junction_coverage, n_samples, rho_source`

This schema may be extended with explicit event identifiers once the splice-event contract is frozen.

### ase_counts

`variant_id, sample_id, panel_gene, ref_count, alt_count, total_depth, is_het, mapping_quality, beta_binomial_rho, rho_stratum`

### variant_annot

`variant_id, panel_gene, consequence, most_severe_consequence, gene_symbol, allele_frequency, gnomad_af, loeuf, pli, mis_z, clinvar_significance, splice_prior, distance_to_splice`

### effect_prior

`mechanism, delta_grid, density, source, n_observations, fit_version`

### power_table

`sample_id, panel_gene, tissue, mechanism, observability, mde, fisher_information, null_evidence_weight, is_hard_zero, baseline, dispersion, coverage_or_depth, effect_star, observability_mean, observability_q05, reason_code, prior_version, n_posterior_draws`

### evidence_table

`sample_id, panel_gene, variant_id, mechanism, z_stat, observability_star, likelihood_ratio, points, acmg_code, decision, mde, capped, provenance_hash`

### graph_nodes / graph_edges

Graph schemas must retain observability, uncertainty, modality, coverage, mapping quality, and tissue mismatch metadata needed for auditability.

## 3. Completeness — LOCKED

For every analysis sample x panel gene x mechanism, exactly one power-table assessment row must exist.

Missing measurement is represented explicitly as a hard-zero row, never by absence of a row.

## 4. Hard-zero reason codes — LOCKED

Expression examples:

- `GENE_SILENT_IN_TISSUE`
- `TISSUE_NOT_IN_REFERENCE`
- `NO_EXPRESSION_RECORD`

Splice examples:

- `NO_JUNCTION_COVERAGE`
- `JUNCTION_NOT_IN_REFERENCE`
- `TISSUE_NOT_IN_REFERENCE`

ASE examples:

- `NOT_HETEROZYGOUS`
- `ZERO_ALLELIC_DEPTH`
- `NO_INFORMATIVE_SITE`

## 5. Provenance — LOCKED

Every processed run must record source URLs/versions, checksums where practical, software versions, random seed, config, and table row counts.

Large files may use recorded size plus an explicit reason if hashing is skipped; confirmatory artifacts should be hashed wherever practical.

## 6. Data-source rules

- GEUVADIS expression: count-scale LCL substrate.
- GEUVADIS ASE: hg19 coordinates must be lifted to GRCh38.
- 1000 Genomes genotype data: region-restricted retrieval for candidate regions; do not download whole genomes/chromosomes unnecessarily.
- GTEx: tissue reference/QC; do not claim GTEx normalized expression supplied count-scale NB dispersion.
- recount3: sample-level expression/junction counts for reference estimation and pretraining; select tissue deliberately.
- gnomAD: panel-restricted annotation/constraint features.
- ClinVar/VEP: version/cache provenance must be recorded.

## 7. Development vs confirmatory artifacts — LOCKED

Keep separate paths/manifests for:

- smoke/development data
- reference-estimation data
- pilot E1 outputs
- locked confirmatory E1 inputs/outputs

Never overwrite the smoke database with an unreviewed reference-model candidate.
