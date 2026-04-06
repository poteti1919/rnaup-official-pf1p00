# RNAup Production Run with `pfScale=1.00`

This repository contains the official production workflow and output organization for generating RNAup-based accessibility features for the 6-nt RNA selector project.

The official RNAup setting adopted in this repository is:

- accessibility mode
- `-u 1-20`
- `-c SHIME`
- `-T 37`
- `--salt 1.021`
- `-d2`
- `--pfScale 1.00`

This setting was selected after validation on both **bpRNA-new** and **CRW**.
For the core accessibility features used in downstream machine-learning workflows, `pfScale=1.00` produced clean runs and preserved transcript-level ranking behavior relative to the previously tested setting.

## Scope

This repository is intended for the **official production run** of RNAup-derived features only.

It is not an exploratory repository for:

- parameter search
- warning investigation
- temporary debugging notebooks
- partial comparison experiments

## Datasets

The production workflow expects the following input files under the `data/` directory:

- `data/bprna_new_test_acgu.csv`
- `data/crw_from_bpseq_acgu.csv`

Each input CSV must contain at least the following columns:

- `id`
- `sequence`
- `structure`

## Main notebook

The official production notebook is:

- `notebooks/RNAup_official_pf1p00_production_complete.ipynb`

This notebook performs the following tasks:

1. Detects the project root.
2. Records the runtime environment.
3. Runs RNAup on CRW and bpRNA-new using the official fixed setting.
4. Saves raw outputs for each sequence.
5. Converts raw outputs into parsed long-format tables.
6. Generates window-level wide-format feature tables.
7. Performs basic QC checks.
8. Writes final acceptance summaries.

## Output structure

The official outputs are written to:

- `results/rnaup_official_pf1p00/`

The directory structure is as follows:

```text
results/rnaup_official_pf1p00/
  artifacts/
    runtime/
      environment_runtime_snapshot.json
      RNAup_version.txt
      conda_env_export.yml
      conda_explicit.txt
      git_revision.txt
      git_status.txt

  CRW/
    meta/
    rnaup_raw/
    rnaup_parsed_long/
    rnaup_parsed_wide/

  bpRNA-new/
    meta/
    rnaup_raw/
    rnaup_parsed_long/
    rnaup_parsed_wide/

  run_summary.csv
  run_summary.txt
  final_assessment.txt
  final_assessment.json
```

## Core output files

For each dataset, the most important files are:

- `meta/rnaup_run_manifest.csv`
- `rnaup_parsed_long/<dataset>/rnaup_long.csv.gz`
- `rnaup_parsed_wide/<dataset>/rnaup_window_features.csv.gz`

The wide-format tables contain the RNAup-derived features that can later be merged into downstream machine-learning tables.

## Quality control

The official production notebook performs the following checks:

- number of input transcripts
- number of processed transcripts
- warning count
- failure count
- missing output count
- missing-value rates for core features such as:
  - `rnaup_open4_S`
  - `rnaup_open6_S`
  - `rnaup_open4_rank_in_transcript`
  - `rnaup_open6_rank_in_transcript`
  - `rnaup_open4_local_z`
  - `rnaup_open6_local_z`

A run is considered clean when:

- all input transcripts are processed
- warning count is zero
- failure count is zero
- missing output count is zero

## Runtime environment

The repository includes environment files under `env/` for reproducibility:

- `env/environment.yml`
- `env/osx-arm64-explicit.txt`

In addition, the notebook writes a runtime snapshot during execution to:

- `results/rnaup_official_pf1p00/artifacts/runtime/`

This runtime snapshot is the authoritative record of the actual execution environment used for the production run.

## Files included in Git

This repository is intended to include:

- notebooks
- environment files
- README
- small summary files
- small QC files
- small metadata files
- parsed result files when their size is manageable

## Raw files not included in Git

Raw RNAup output files are **not uploaded to Git** because they are large and can easily clutter the repository.

Examples include:

- per-sequence raw output directories under `rnaup_raw/`
- raw RNAup logs for every sequence
- other large intermediate files that are not necessary for normal reuse of the processed outputs

These raw files should be kept locally or distributed separately when needed.

## Notes on public release

This repository is intended to contain the clean production workflow and the official output organization.

Exploratory notebooks, parameter screening notebooks, and temporary debugging files should be kept outside this repository or in a separate development repository.

Raw RNAup files are not included in Git because they are large.

Selected small result files and metadata are included in the repository.
Large wide-format result tables may be tracked via Git LFS or distributed separately as release assets.

## Citation / usage note

If this repository is used in a manuscript, please cite it as the official RNAup production workflow used to generate accessibility-derived features under the fixed condition `pfScale=1.00`.

