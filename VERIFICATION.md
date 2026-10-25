# Checks and reproducibility

The suite has 40 tests covering battery constraints, empirical CVaR, configured risk levels, shared scenario ranks, input validation, output paths, required CLI arguments, PDF link targets and fresh versus reused study results.

Local check status, environment and scope are recorded in
[`verification/current_checks.json`](verification/current_checks.json), with the
[unit-test output](verification/unit_tests.txt) and
[release-verification output](verification/release_verification.txt). These local
records are separate from this repository's CI.

From the repository root:

```text
python -m pip install -r requirements-lock.txt
python -m unittest discover -s tests -v
python scripts/verify_release.py
```

The release verifier reruns three reference risk weights and compares six tables per run: 18 comparisons at absolute tolerance 1e-8. It checks 3,486 saved and rerun schedules for the full battery constraints, using temporary output and leaving supplied reference results unchanged.

The reference environment uses Python 3.13.15, NumPy 2.3.5, pandas 2.2.3,
SciPy 1.17.0, Matplotlib 3.10.8 and PyYAML 6.0.3, matching
`requirements-lock.txt`. On Windows, use a short checkout path because the
verifier appends temporary output and figure paths.

## Supplementary results

`corrected_v110/supplementary_20261021/` contains paired N=60/200 results and the idle baseline. Its [recorded study manifest](corrected_v110/supplementary_20261021/study_manifest.json) reports 996 strategy-day schedules and 83 idle schedules: 25,896 periods with zero full-constraint violations. The result files retain all 12 metric groups, six paired comparisons and 249 recorded sampling prefixes. The current release verification does not rerun the full N=60/200 optimization study or replace its recorded solve timings. For fresh execution, use the [README commands](README.md#reproduce).

## This repository's checks

The [GitHub Actions page](https://github.com/KeithCai1995/storage-decision-portfolio/actions) reports checks for this repository's commits. The workflow runs on Ubuntu and Windows with Python 3.11, 3.12 and 3.13; every job runs the tests and release verifier. Check the result associated with the distributed commit.

Experiment run manifests retain their actual execution dates and source hashes.
They are not inventories of the current distribution; [PUBLICATION.md](PUBLICATION.md)
explains that distinction.
